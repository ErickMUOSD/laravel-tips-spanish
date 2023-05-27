 ## Routing

⬆️ [Menú principal](README.md#laravel-tips) ⬅️ [Anterior (Views)](views.md) ➡️ [Siguiente (Validation)](validation.md)

- [Route group within a group](#route-group-within-a-group)
- [Declare a resolveRouteBinding method in your Model](#declare-a-resolveroutebinding-method-in-your-model)
- [assign withTrashed() to Route::resource() method](#assign-withtrashed-to-routeresource-method)
- [Skip Input Normalization](#skip-input-normalization)
- [Wildcard subdomains](#wildcard-subdomains)
- [What's behind the routes?](#whats-behind-the-routes)
- [Route Model Binding: You can define a key](#route-model-binding-you-can-define-a-key)
- [Route Fallback: When no Other Route is Matched](#route-fallback-when-no-other-route-is-matched)
- [Route Parameters Validation with RegExp](#route-parameters-validation-with-regexp)
- [Rate Limiting: Global and for Guests/Users](#rate-limiting-global-and-for-guestsusers)
- [Query string parameters to Routes](#query-string-parameters-to-routes)
- [Separate Routes by Files](#separate-routes-by-files)
- [Translate Resource Verbs](#translate-resource-verbs)
- [Custom Resource Route Names](#custom-resource-route-names)
- [Eager load relationship](#eager-load-relationship)
- [Localizing Resource URIs](#localizing-resource-uris)
- [Resource Controllers naming](#resource-controllers-naming)
- [Easily highlight your navbar menus](#easily-highlight-your-navbar-menus)
- [Generate absolute path using route() helper](#generate-absolute-path-using-route-helper)
- [Override the route binding resolver for each of your models](#override-the-route-binding-resolver-for-each-of-your-models)
- [If you need public URL, but you want them to be secured](#if-you-need-public-url-but-you-want-them-to-be-secured)
- [Using Gate in middleware method](#using-gate-in-middleware-method)
- [Simple route with arrow function](#simple-route-with-arrow-function)
- [Route view](#route-view)
- [Route directory instead of route file](#route-directory-instead-of-route-file)
- [Route resources grouping](#route-resources-grouping)
- [Custom route bindings](#custom-route-bindings)
- [Two ways to check the route name](#two-ways-to-check-the-route-name)
- [Route model binding soft-deleted models](#route-model-binding-soft-deleted-models)
- [Retrieve the URL without query parameters](#retrieve-the-url-without-query-parameters)
- [Customizing Missing Model Behavior in route model bindings](#customizing-missing-model-behavior-in-route-model-bindings)
- [Exclude middleware from a route](#exclude-middleware-from-a-route)
- [Controller groups](#controller-groups)

### Route group within a group

In Routes, you can create a group within a group, assigning a certain middleware only to some URLs in the "parent" group.

```php
Route::group(['prefix' => 'account', 'as' => 'account.'], function() {
    Route::get('login', [AccountController::class, 'login']);
    Route::get('register', [AccountController::class, 'register']);
    Route::group(['middleware' => 'auth'], function() {
        Route::get('edit', [AccountController::class, 'edit']);
    });
});
```

### Declare a resolveRouteBinding method in your Model

Route model binding proveé una conveniente manera de injéctar instacias de nuestros Models directamente en las rutas, esto es bastante util, pero hay casos donde no podemos permitir fácilmente el acceso mediante ID's. Quizá necesitamos verificar algunas cosas más.

```php
public function resolveRouteBinding($value, $field = null)
{
     $user = request()->user();

     return $this->where([
          ['user_id' => $user->id],
          ['id' => $value]
     ])->firstOrFail();
}
```

⭐ Aportación de  [@notdylanv](https://twitter.com/notdylanv/status/1567296232183447552/)

### assign withTrashed() to Route::resource() method

Antes de Laravel 9.35 para buscar usuarios eliminados lógicamente debíamos hacer esto:
```php
Route::get('/users/{user}', function (User $user) {
     return $user->email;
})->withTrashed();
```

Desde Laravel 9.35 podemos asignar a un resource.
```php
Route::resource('users', UserController::class)
     ->withTrashed();
```

Incluso podemos determinar los métodos.
```php
Route::resource('users', UserController::class)
     ->withTrashed(['show']);
```

### Skip Input Normalization

Laravel automáticamente elimina los espacios en todos los strings de las peticiones. Es llamado input normalización.

Aveces, quizá no querrás este comportamiento.

Para esto puedes usar el método `skipWhen` en el middleware TrimString y retornar true para saltarlo. 
```php
public function boot()
{
     TrimStrings::skipWhen(function ($request) {
          return $request->is('admin/*);
     });
}
```

⭐ Aportación de  [@Laratips1](https://twitter.com/Laratips1/status/1580210517372596224)

### Wildcard subdomains

Puedes crear un grupo de rutas por cada nombre de subdominio dinámico y pasar le su valor en cada ruta,

```php
Route::domain('{username}.workspace.com')->group(function () {
    Route::get('user/{id}', function ($username, $id) {
        //
    });
});
```

### What's behind the routes?

Si ocupas el paquete [Laravel UI package](https://github.com/laravel/ui), probablemente quieras saber que rutas hay detrás de   `Auth::routes()`.

```php
public function auth()
{
    return function ($options = []) {
        // Authentication Routes...
        $this->get('login', 'Auth\LoginController@showLoginForm')->name('login');
        $this->post('login', 'Auth\LoginController@login');
        $this->post('logout', 'Auth\LoginController@logout')->name('logout');
        // Registration Routes...
        if ($options['register'] ?? true) {
            $this->get('register', 'Auth\RegisterController@showRegistrationForm')->name('register');
            $this->post('register', 'Auth\RegisterController@register');
        }
        // Password Reset Routes...
        if ($options['reset'] ?? true) {
            $this->resetPassword();
        }
        // Password Confirmation Routes...
        if ($options['confirm'] ?? class_exists($this->prependGroupNamespace('Auth\ConfirmPasswordController'))) {
            $this->confirmPassword();
        }
        // Email Verification Routes...
        if ($options['verify'] ?? false) {
            $this->emailVerification();
        }
    };
}
```

La linea que nos trae todo esto no tiene parámetros.
```php
Auth::routes(); // no parameters
```

Pero puedes proveer parámetros para habilitar o invalidar ciertas rutas. 
```php
Auth::routes([
    'login'    => true,
    'logout'   => true,
    'register' => true,
    'reset'    => true,  // for resetting passwords
    'confirm'  => false, // for additional password confirmations
    'verify'   => false, // for email verification
]);
```


⭐ Tip basado en la [sugerencia](https://github.com/LaravelDaily/laravel-tips/pull/57) de  [MimisK13](https://github.com/MimisK13)

### Route Model Binding: You can define a key

Puedes Route model binding de la siguiente manera: `Route::get('api/users/{user}', function (User $user) { … }`, pero no solo por el campo de ID. Si deseas que `{user}` sea el campo `username`, coloca lo siguiente en el modelo:
```php
public function getRouteKeyName() {
    return 'username';
}
```

### Route Fallback: When no Other Route is Matched

Si quieres especificar una lógica adicional para rutas no encontradas, en vez de solo lanzar la página 403, puedes crear una ruta especial para eso, al final de las rutas agrega esto:
```php
Route::group(['middleware' => ['auth'], 'prefix' => 'admin', 'as' => 'admin.'], function () {
    Route::get('/home', [HomeController::class, 'index']);
    Route::resource('tasks', [Admin\TasksController::class]);
});

// Some more routes....
Route::fallback(function() {
    return 'Hm, why did you land here somehow?';
});
```

### Route Parameters Validation with RegExp

Podemos validar parámetros directamente en la ruta con el parámetro "where". Un caso bastante típico es colocar un prefijo en las rutas por idioma local como: `es/blog` and `en/article/333`.  Para asegurarnos de esto hacemos:  

`routes/web.php`:

```php
Route::group([
    'prefix' => '{locale}',
    'where' => ['locale' => '[a-zA-Z]{2}']
], function () {
    Route::get('/', [HomeController::class, 'index']);
    Route::get('article/{id}', [ArticleController::class, 'show']);;
});
```

### Rate Limiting: Global and for Guests/Users

Podemos limitar el máximo de llamadas de una URL de 60 veces por minuto con `throttle:60,1`:
```php
Route::middleware('auth:api', 'throttle:60,1')->group(function () {
    Route::get('/user', function () {
        //
    });
});
```

Pero también podemos hacerlo separadamente por público y para personas logeadas.
```php
// maximum of 10 requests for guests, 60 for authenticated users
Route::middleware('throttle:10|60,1')->group(function () {
    //
});
```


 Además puedes tener un campo  rate_limit en la tabla usuarios para tomar la cantidad:
```php
Route::middleware('auth:api', 'throttle:rate_limit,1')->group(function () {
    Route::get('/user', function () {
        //
    });
});
```

### Query string parameters to Routes


Si pasas adicionales parámetros a una ruta, esas llaves y valores serán automáticamente añadidas a la ruta generada.

```php
Route::get('user/{id}/profile', function ($id) {
    //
})->name('profile');

$url = route('profile', ['id' => 1, 'photos' => 'yes']); // Result: /user/1/profile?photos=yes
```

### Separate Routes by Files

Si tienes  rutas relacionadas a ciertos módulos, puedes separarlas en un archivo  `routes/XXXXX.php`  e incluirla en el archivo `routes/web.php`.

Un ejemplo con las rutas de  [Laravel Breeze](https://github.com/laravel/breeze/blob/1.x/stubs/routes/web.php) :
```php
Route::get('/', function () {
    return view('welcome');
});

Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware(['auth'])->name('dashboard');

require __DIR__.'/auth.php';
```

Tu archivo  `routes/auth.php`:  quedaría así:
```php
use App\Http\Controllers\Auth\AuthenticatedSessionController;
use App\Http\Controllers\Auth\RegisteredUserController;
// ... more controllers

use Illuminate\Support\Facades\Route;

Route::get('/register', [RegisteredUserController::class, 'create'])
                ->middleware('guest')
                ->name('register');

Route::post('/register', [RegisteredUserController::class, 'store'])
                ->middleware('guest');

// ... A dozen more routes
```

Pero debería usar `include()`  solo cuando ese archivo separado tiene las mismas configuración para el prefijo/middlewares de otra manera es mejor agruparlas en `app/Providers/RouteServiceProvider`:
```php
public function boot()
{
    $this->configureRateLimiting();

    $this->routes(function () {
        Route::prefix('api')
            ->middleware('api')
            ->namespace($this->namespace)
            ->group(base_path('routes/api.php'));

        Route::middleware('web')
            ->namespace($this->namespace)
            ->group(base_path('routes/web.php'));

        // ... Your routes file listed next here
    });
}
```

### Translate Resource Verbs

Si usas controladores tipo resource, pero quieres cambiar los verbos que aparecen en la URL a otro idioma para propósitos de SEO, en vez de que aparezca   `/create`  aparecerá así: `/crear`, para configurar lo coloca este código en el método boot de `App\Providers\RouteServiceProvider`:
```php
public function boot()
{
    Route::resourceVerbs([
        'create' => 'crear',
        'edit' => 'editar',
    ]);

    // ...
}
```

### Custom Resource Route Names

Cuando usamos controladores tipo resources in  `routes/web.php`  puedes especificar los nombres de las rutas, así el prefijo de la URL en el navegador y el prefijo de la ruta puede ser diferente.
```php
Route::resource('p', ProductController::class)->names('products');
```


Este código generará URL como  `/p`, `/p/{id}`, `/p/{id}/edit`, . Pero para llamarlos como nombre de ruta:  `route('products.index')`, `route('products.create')`, etc.

### Eager load relationship

Si usas la función de Route Model Binding y piensas que no puedes usar   Eager Loading para relaciones, piénsalo de nuevo
```php
public function show(Product $product) {
    //
}
```

Aunque tengas relaciones tipo belongsTo, puedes usar la función `->load()`
```php
public function show(Product $product) {
    $product->load('category');
    //
}
```

### Localizing Resource URIs

Si utilizas controladores tipo resource, pero quieres cambiar los verbos de la URL a otro idioma. En vez de que la ruta aparezca así `/create` debería aparecer  en español `/crear`. Para esto debes configurar los en el método `Route::resourceVerbs()` .
```php
public function boot()
{
    Route::resourceVerbs([
        'create' => 'crear',
        'edit' => 'editar',
    ]);
    //
}
```

### Resource Controllers naming

En los controladores de recursos (Resource Controllers), en `routes/web.php`, puedes especificar el parámetro `->names()`, de modo que el prefijo de la URL y el prefijo del nombre de la ruta puedan ser diferentes.

Esto generará URLs como `/p`, `/p/{id}`, `/p/{id}/edit`, etc. Pero podrías llamarlos de la siguiente manera:

- `route('products.index)`
- `route('products.create)`
- etc.

```php
Route::resource('p', \App\Http\Controllers\ProductController::class)->names('products');
```

### Easily highlight your navbar menus

Usa el   `Route::is('route-name')` para fácilmente  resaltar tus menus navbar.
```blade
<ul>
    <li @if(Route::is('home')) class="active" @endif>
        <a href="/">Home</a>
    </li>
    <li @if(Route::is('contact-us')) class="active" @endif>
        <a href="/contact-us">Contact us</a>
    </li>
</ul>
```
 

⭐ Aportación de [@anwar_nairi](https://twitter.com/anwar_nairi/status/1443893957507747849)

### Generate absolute path using route() helper

```php
route('page.show', $page->id);
// http://laravel.test/pages/1

route('page.show', $page->id, false);
// /pages/1
```


⭐ Aportación de [@oliverds\_](https://twitter.com/oliverds_/status/1445796035742240770)

### Override the route binding resolver for each of your models

Puedes sobrescribir  el route binding  para cada uno de tus modelos. En este ejemplo, no tengo control sobre el signo @ en la URL, por lo que utilizando el método `resolveRouteBinding`, puedo eliminar el signo @ y resolver el modelo.
```php
// Ruta 
Route::get('{product:slug}', Controller::class);

// Petición
https://nodejs.pub/@unlock/hello-world

// Modelo Producto
public function resolveRouteBinding($value, $field = null)
{
    $value = str_replace('@', '', $value);

    return parent::resolveRouteBinding($value, $field);
}
```


⭐ Aportación de  [@Philo01](https://twitter.com/Philo01/status/1447539300397195269)

### If you need public URL, but you want them to be secured


Si necesitas una URL pública pero quieres mantener la asegurada, usa Laravel signed URL.
```php
class AccountController extends Controller
{
    public function destroy(Request $request)
    {
        $confirmDeleteUrl = URL::signedRoute('confirm-destroy', [
            $user => $request->user()
        ]);
        // Send link by email...
    }

    public function confirmDestroy(Request $request, User $user)
    {
        if (! $request->hasValidSignature()) {
            abort(403);
        }

        // User confirmed by clicking on the email
        $user->delete();

        return redirect()->route('home');
    }
}
```


⭐ Aportación de  [@anwar_nairi](https://twitter.com/anwar_nairi/status/1448239591467589633)

### Using Gate in middleware method
  
Puedes utilizar las puertas (gates) que especificaste en `App\Providers\AuthServiceProvider` en el método de middleware.

Para hacer esto, solo necesitas colocar dentro de `can:` los nombres de las puertas necesarias.
```php
Route::put('/post/{post}', function (Post $post) {
    // The current user may update the post...
})->middleware('can:update,post');
```

### Simple route with arrow function

Puedes usar una función de flecha en las rutas, sin tener que usar una función anónima.

Para hacer esto, solo tienes que usar  `fn() =>`.
```php
// Instead of
// En vez de esto
Route::get('/example', function () {
    return User::all();
});

// You can
// Pudes usar 
Route::get('/example', fn () => User::all());
```

### Route view

Puedes definir  `Route::view($uri , $bladePage)` para retornar una vista directamente, sin tener que usar un controlador.
```php
//this will return home.blade.php view
Route::view('/home', 'home');
```

### Route directory instead of route file

También puedes crear un directorio de rutas  así:_/routes/web/_, para tener   más organización y solo llamar en el archivo web.php lo siguiente: 
```php
foreach(glob(dirname(__FILE__).'/web/*', GLOB_NOSORT) as $route_file){
    include $route_file;
}
```

Ahora cada archivo dentro de /routes/web/ actuará como un archivo router web donde puedes tener diferentes rutas en muchos archivos.

### Route resources grouping

Si en tus rutas tienes muchos controladores tipo resources, puedes agruparlos y llamar los en   Route::resources() en vez de definir cada ruta por igual.
```php
Route::resources([
    'photos' => PhotoController::class,
    'posts' => PostController::class,
]);
```

### Custom route bindings

¿Sabías que puedes definir route bindings personalizados?

En este ejemplo, necesito buscar un portafolio por slug. Pero el slug no es único, porque multiples usuarios pueden tener un portafolio llamado 'Foo'. Así que defino como Laravel puede buscar lo desde un parámetro de la ruta.
```php
class RouteServiceProvider extends ServiceProvider
{
    public const HOME = '/dashboard';

    public function boot()
    {
        Route::bind('portfolio', function (string $slug) {
            return Portfolio::query()
                ->whereBelongsto(request()->user())
                ->whereSlug($slug)
                ->firstOrFail();
        });
    }
}
```

```php
Route::get('portfolios/{portfolio}', function (Portfolio $portfolio) {
    /*
     * The $portfolio will be the result of the query defined in the RouteServiceProvider
 * El $portafolio será el resultado de la  busqueda definida en el RouteServiceProvider.
     */
})
```


⭐Aportación de [@mmartin_joo](https://twitter.com/mmartin_joo/status/1496871240346509312)

### Two ways to check the route name

Aquí tenemos dos maneras de revisar el nombre de la ruta en Laravel.
```php
// #1
<a
    href="{{ route('home') }}"
    @class="['navbar-link', 'active' => Route::current()->getName() === 'home']"
>
    Home
</a>
// #2
<a
    href="{{ route('home') }}"
    @class="['navbar-link', 'active' => request()->routeIs('home)]"
>
    Home
</a>
```


⭐Aportación de [@AndrewSavetchuk](https://twitter.com/AndrewSavetchuk/status/1510197418909999109)

### Route model binding soft-deleted models

Por defecto, cuando usamos el route model binding no podremos traer modelos que han sido borrados con el soft-delete. Para evitar este comportamiento debemos usar el método   `withTrashed`  en la ruta.
```php
Route::get('/posts/{post}', function (Post $post) {
    return $post;
})->withTrashed();
```

⭐Aportación de  [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1511154599255703553)

### Retrieve the URL without query parameters

SI por alguna razón, tú URL tiene query parameters, puedes remover esos query parameters usando el método  `fullUrlWithoutQuery`  sobre la petición así:
```php
// Original  URL: https://www.amitmerchant.com?search=laravel&lang=en&sort=desc
$urlWithQueryString = $request->fullUrlWithoutQuery([
    'lang',
    'sort'
]);
echo $urlWithQueryString;
// Salida:  https://www.amitmerchant.com?search=laravel
```


⭐Aportación de [@amit_merchant](https://twitter.com/amit_merchant/status/1510867527962066944)

### Customizing Missing Model Behavior in route model bindings

Por defecto, Laravel lanza un excepción 404 cuando no puede hacer un bind al modelo, puedes cambiar el comportamiento pasando le una función al método missing.
```php
Route::get('/users/{user}', [UsersController::class, 'show'])
    ->missing(function ($parameters) {
        return Redirect::route('users.index');
    });
```


⭐Aportación de  [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1511322007576608769)

### Exclude middleware from a route

Puedes excluir middlewares de cualquier ruta usando el método `withoutMiddleware`.
```php
Route::post('/some/route', SomeController::class)
    ->withoutMiddleware([VerifyCsrfToken::class]);
```


⭐ Aportación de [@alexjgarrett](https://twitter.com/alexjgarrett/status/1512529798790320129)

### Controller groups

En vez de usar el controlador en cada ruta, puedes considerar usar un route controller group. Este fue añadido desde la versión 8.80.

```php
// Antes
Route::get('users', [UserController::class, 'index']);
Route::post('users', [UserController::class, 'store']);
Route::get('users/{user}', [UserController::class, 'show']);
Route::get('users/{user}/ban', [UserController::class, 'ban']);
// Después
Route::controller(UsersController::class)->group(function () {
    Route::get('users', 'index');
    Route::post('users', 'store');
    Route::get('users/{user}', 'show');
    Route::get('users/{user}/ban', 'ban');
});
```

⭐ Aportación de  [@justsanjit](https://twitter.com/justsanjit/status/1514943541612527616)
