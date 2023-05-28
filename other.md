## Other

⬆️ [Menú principal](README.md#laravel-tips) ⬅️ [Anterior (API)](api.md)

- [Localhost in .env](#localhost-in-env)
- [Time value in the past/future](#time-value-in-the-pastfuture)
- [Do some work after a response has been sent to the browser](#do-some-work-after-a-response-has-been-sent-to-the-browser)
- [Redirect with URL fragment](#redirect-with-url-fragment)
- [Use middleware to adjust incoming request](#use-middleware-to-adjust-incoming-request)
- [Redirect away from Laravel app](#redirect-away-from-laravel-app)
- [Blade directive to show data in specific environment](#blade-directive-to-show-data-in-specific-environment)
- [Schedule Laravel job based on time zone](#schedule-laravel-job-based-on-time-zone)
- [Use assertModelMissing instead assertDatabaseMissing](#use-assertmodelmissing-instead-assertdatabasemissing)
- [Various options to format diffForHumans()](#various-options-to-format-diffforhumans)
- [Create custom disks at runtime](#create-custom-disks-at-runtime)
- [When (NOT) to run "composer update"](#when-not-to-run-composer-update)
- [Composer: Check for Newer Versions](#composer-check-for-newer-versions)
- [Auto-Capitalize Translations](#auto-capitalize-translations)
- [Carbon with Only Hours](#carbon-with-only-hours)
- [Single Action Controllers](#single-action-controllers)
- [Redirect to Specific Controller Method](#redirect-to-specific-controller-method)
- [Use Older Laravel Version](#use-older-laravel-version)
- [Add Parameters to Pagination Links](#add-parameters-to-pagination-links)
- [Repeatable Callback Functions](#repeatable-callback-functions)
- [Request: has any](#request-has-any)
- [Simple Pagination](#simple-pagination)
- [Blade directive to add true/false conditions](#blade-directive-to-add-truefalse-conditions)
- [Jobs can be used without queues](#jobs-can-be-used-without-queues)
- [Use faker outside factories or seeders](#use-faker-outside-factories-or-seeders)
- [Schedule things](#schedule-things)
- [Search Laravel docs](#search-laravel-docs)
- [Filter route:list](#filter-routelist)
- [Blade directive for not repeating yourself](#blade-directive-for-not-repeating-yourself)
- [Artisan commands help](#artisan-commands-help)
- [Disable lazy loading when running your tests](#disable-lazy-loading-when-running-your-tests)
- [Using two amazing helpers in Laravel will bring magic results](#using-two-amazing-helpers-in-laravel-will-bring-magic-results)
- [Request parameter default value](#request-parameter-default-value)
- [Pass middleware directly into the route without register it](#pass-middleware-directly-into-the-route-without-register-it)
- [Transforming an array to CssClasses](#transforming-an-array-to-cssclasses)
- ["upcomingInvoice" method in Laravel Cashier (Stripe)](#upcominginvoice-method-in-laravel-cashier-stripe)
- [Laravel Request exists() vs has()](#laravel-request-exists-vs-has)
- [There are multiple ways to return a view with variables](#there-are-multiple-ways-to-return-a-view-with-variables)
- [Schedule regular shell commands](#schedule-regular-shell-commands)
- [HTTP client request without verifying](#http-client-request-without-verifying)
- [Test that doesn't assert anything](#test-that-doesnt-assert-anything)
- ["Str::mask()" method](#strmask-method)
- [Extending Laravel classes](#extending-laravel-classes)
- [Can feature](#can-feature)
- [Temporary download URLs](#temporary-download-urls)
- [Dealing with deeply-nested arrays](#dealing-with-deeply-nested-arrays)
- [Customize how your exceptions are rendered](#customize-how-your-exceptions-are-rendered)
- [The tap helper](#the-tap-helper)
- [Reset all of the remaining time units](#reset-all-of-the-remaining-time-units)
- [Scheduled commands in the console kernel can automatically email their output if something goes wrong](#scheduled-commands-in-the-console-kernel-can-automatically-email-their-output-if-something-goes-wrong)
- [Be careful when constructing your custom filtered queries using GET parameters](#be-careful-when-constructing-your-custom-filtered-queries-using-get-parameters)
- [Dust out your bloated route file](#dust-out-your-bloated-route-file)
- [You can send e-mails to a custom log file](#you-can-send-e-mails-to-a-custom-log-file)
- [Markdown made easy](#markdown-made-easy)
- [Simplify if on a request with whenFilled() helper](#simplify-if-on-a-request-with-whenfilled-helper)
- [Pass arguments to middleware](#pass-arguments-to-middleware)
- [Get value from session and forget](#get-value-from-session-and-forget)
- [$request->date() method](#request-date-method)
- [Use through instead of map when using pagination](#use-through-instead-of-map-when-using-pagination)
- [Quickly add a bearer token to HTTP request](#quickly-add-a-bearer-token-to-http-request)
- [Copy file or all files from a folder](#copy-file-or-all-files-from-a-folder)
- [Sessions has() vs exists() vs missing()](#sessions-has-vs-exists-vs-missing)
- [Test that you are passing the correct data to a view](#test-that-you-are-passing-the-correct-data-to-a-view)
- [Use Redis to track page views](#use-redis-to-track-page-views)
- [to_route() helper function](#to_route-helper-function)
- [Pause a long running job when queue worker shuts down](#pause-a-long-running-job-when-queue-worker-shuts-down)
- [Freezing Time in Laravel Tests](#freezing-time-in-laravel-tests)
- [New squish helper](#new-squish-helper)
- [Specify what to do if a scheduled task fails or succeeds](#specify-what-to-do-if-a-scheduled-task-fails-or-succeeds)
- [Scheduled command on specific environments](#scheduled-command-on-specific-environments)
- [Add conditionable behavior to your own classes](#add-conditionable-behavior-to-your-own-classes)
- [Perform Action when Job has failed](#perform-action-when-job-has-failed)

### Localhost in .env

No olvides cambiar tu `APP_URL` in tu  archivo `.env` de   `http://localhost`  a  la url real. Esta URL es la base para cualquier external link en notificaciones, emails y otras cosas. 

```
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:9PHz3TL5C4YrdV6Gg/Xkkmx9btaE93j7rQTUZWm2MqU=
APP_DEBUG=true
APP_URL=http://localhost
```

### Time value in the past/future


Si quieres usar algún valor de tiempo como pasado/futuro, puedes construirlo usando los helpers de Laravel/Carbon de esta manera: 
```php
$product = Product::factory()->create([
     'published_at' => now()->addDay()->setTime(14, 00),
]);
```

### Do some work after a response has been sent to the browser

También puedes añadir lógica a los  middlwares después de mandar una respuesta al navegador.

Para esto debes añadir el método `terminate` en ell middleware. Este método será automaticamente llamado despues de enviar la respuesta al navegador. Utiliza los mismos parametros que el método `handle()`
```php
class TerminatingMiddleware
{
    public function handle($request, Closure $next)
    {
        return $next($request);
    }
 
    public function terminate($request, $response)
    {
        // ...
    }
}
```


⭐ Aportación de  [@Laratips1](https://twitter.com/Laratips1/status/1567045288338280448/)

### Redirect with URL fragment

¿Sabías que puedes añadir un fragmento de URL extra cuando redireccionas a una ruta en Laravel?

Es bastante util cuando se trata de redireccionar a una sección específica de la pagina por ejemplo: Las sección de reviews en un producto.
```php
return redirect()
     ->back()
     ->withFragment('contactForm');
     // domain.test/url#contactForm

return redirect()
     ->route('product.show')
     ->withFragment('reviews');
     // domain.test/product/23#reviews
```

⭐ Aportación de   [@ecrmnn](https://twitter.com/ecrmnn/status/1574813643425751040)

### Use middleware to adjust incoming request

Los middlewares sirven de muchas maneras como modificar el contenido de peticiones entrantes. Por ejemplo:  decidí renombrar un modelo en mi aplicación, en vez de añadir una nueva versión de API para un cambio importante, simplemente puedo convertir esas peticiones usando una referencia.
```php
class ConvertLicenseeIntoContact
{
     public function handle(Request $request, Closure $next)
     {
          if($request->json('licensee_id')) {
               $request->json()->set('contact_id', $request->json('licensee_id'));
          }

          return $next($request);
     }
}
```

⭐ Aportación de  [@Philo01](https://twitter.com/Philo01/status/1581214787467235329)

### Redirect away from Laravel app

Algunas veces necesitas redireccionar fuera de tu aplicación, para estos casos existe el método **away** que puedes llamarlo junto con el método  **redirect()**.
```php
redirect()->away('https://www.google.com');
```


Esto crea un  `RedirectResponse`  sin adicionales URL encoding, validaciones o verificaciones.

⭐ Aportación de  [@Laratips1](https://twitter.com/Laratips1/status/1581887837972361216)

### Blade directive to show data in specific environment

¿Sabías que Laravel Blade tiene directivas como 'Production' que puedes usar para mostrar información cuando tu entorni es Producción?

Hay también otra directica 'env' que puedes usar para mostrar información en un entorno(production, staging, development) específico.
```blade
@production
     // I am only visible to the production environment...
     // Soy visible en el entorno production
@endproduction

@env('staging')
     // I am only visible to the staging environment...
     // Soy solo visible en el enotrni staging
@endenv

@env(['staging', 'production'])
     // I am only visible to both the staging and production environment...
     // Soy solo visible ambos entornos staging y production
@endenv
```

⭐ Aportación de  [@Laratips1](https://twitter.com/Laratips1/status/1581887837972361216)

### Schedule Laravel job based on time zone

¿Sabías que puedes programar Laravel jobs basados en la zona horaria?

```php
$schedule->command('reportg:generate')
         ->timezone('America/New_York')
         ->at('2:00');
```


Si en repetidas ocasiones asignas la misma zona horaria en todas tus tareas programadas, puedes definir el método  `scheduleTimezone`  en la clase `app\Console\Kernel` 
```php
protected function scheduleTimezone()
{
     return 'America/Chicago';
}
```

⭐ Aportación de [@binumathew](https://twitter.com/binumathew/status/1584830693791928320)

### Use assertModelMissing instead assertDatabaseMissing


Mientras realizamos pruebas tipo borrado de modelos, usa `assertModelMissing` en vez de `assertDatabaseMissing`
```php
/** @test */
public function allowed_user_can_delete_task()
{
     $task = Task::factory()->for($this->project)->create();

     $this->deleteJson($task->path())
          ->assertNoContent();

     // Instead of assertDatabaseMissing to check if the model missing from the database
     // En vez de usar assertDatabaseMissing para verificar si el modelo no está en la base de datos.
     $this->assertDatabaseMissing('tasks', ['id' => $task->id]);

     // use directly assertModelMissing 
     //usa directamente assertModelMissing
     $this->assertModelMissing($task);
}
```


⭐ Aportación de [@h_ik04](https://twitter.com/h_ik04/status/1585593621193129986)

### Various options to format diffForHumans()

En Laravel/Carbon, ¿Sabías que puedes añadir varias opciones de formato para que las fechas sean leídas sin complicaciones? [Mira la documentación completa para más ejemplos](https://carbon.nesbot.com/docs/#api-humandiff)
```php
$user->created_at->diffForHumans();
```
=> `"17 hours ago"`


```php
$user->created_at->diffForHumans([
     'parts' => 2
]);
```
=> `"17 hours 54 minutes ago"`


```php
$user->created_at->diffForHumans([
     'parts' => 3
     'join' => ', ',
]);
```
=> `"17 hours, 54 minutes, 50 seconds ago"`


```php
$user->created_at->diffForHumans([
     'parts' => 3,
     'join' => ', ',
     'short' => true,
]);
```
=> `"17h, 54m, 50s ago"`

### Create custom disks at runtime

¿Sabías que puedes crear discos personalizados en tiempo de ejecución sin necesidad de tener un archivo config/filesystems.file ?

Esto puede ser útil para administrar archivos en rutas personalizadas sin la necesidad de agregarlas a la configuración.
```php
$avatarDisk = Storage::build([
    'driver' => 'local',
    'root' => storage_path('app/avatars'),
]);
$avatarDisk->put('user_avatar.jpg', $image);
```

⭐ Aportación de [@wendell_adriel](https://twitter.com/wendell_adriel/)

### When (NOT) to run "composer update"

Esto no está tan relacionado a Laravel, pero nunca corras el comando `composer update`  en producción, es bastante lento y  puede romper las dependecias. SIempre corre `composer update` localmente, sube el commit con los cambios  del `composer.lock`  y después en  producción corre `composer install`.

### Composer: Check for Newer Versions

Si quieres saber cuando los paquetes del  `composer.json`  han lanzado una nueva versión, solo corre el compando   `composer outdated`. Este lanzará una lista con toda la información de los paquetes que necesitan actualizarse:
```
phpdocumentor/type-resolver 0.4.0 0.7.1
phpunit/php-code-coverage   6.1.4 7.0.3 Library that provides collection, processing, and rende...
phpunit/phpunit             7.5.9 8.1.3 The PHP Unit Testing framework.
ralouphie/getallheaders     2.0.5 3.0.3 A polyfill for getallheaders.
sebastian/global-state      2.0.0 3.0.0 Snapshotting of global state
```

### Auto-Capitalize Translations

En los archivos de traducciones `resources/lang`, puedes especificar no solo variables así `:variable`, sino también capitalizarlas así `:Variable` o `:VARIABLE` a el valor que le pases este será capitalizado automaticamente.
```php
// resources/lang/en/messages.php
'welcome' => 'Welcome, :Name'

// Result: "Welcome, Taylor"
echo __('messages.welcome', ['name' => 'taylor']);
```

### Carbon with Only Hours


Si quieres obtener la fecha actual sin segundos y/o minutos, usa los métodos `setSeconds(0)` o  `setMinutes(0)` de Carbon. 

```php
// 2020-04-20 08:12:34
echo now();

// 2020-04-20 08:12:00
echo now()->setSeconds(0);

// 2020-04-20 08:00:00
echo now()->setSeconds(0)->setMinutes(0);

// Another way - even shorter
echo now()->startOfHour();
```

### Single Action Controllers

Si necesitas crear un controlador para manejar una sola acción puedes usar el método  `__invoke()`  y además crear un controlador  "invokable" .

Ruta:

```php
Route::get('user/{id}', ShowProfile::class);
```

Artisan:

```bash
php artisan make:controller ShowProfile --invokable
```

Controller:

```php
class ShowProfile extends Controller
{
    public function __invoke($id)
    {
        return view('user.profile', [
            'user' => User::findOrFail($id)
        ]);
    }
}
```

### Redirect to Specific Controller Method

Con el método  `redirect()` no solo puedes  redireccionar a una url en específco o nombre de la ruta, sino también a un controlador específico en cualquier método e incluso pasarle parametros.

```php
return redirect()->action([SomeController::class, 'method'], ['param' => $value]);
```

### Use Older Laravel Version

Si quieres usar una version más antigua en vez de la ultima usa este comando:

```bash
composer create-project --prefer-dist laravel/laravel project "7.*"
```

Cambiar 7:* por la versión que quieras

### Add Parameters to Pagination Links

In default Pagination links, you can pass additional parameters, preserve the original query string, or even point to a specific `#xxxxx` anchor.

Por defecto en los  links de paginación puedes pasar parámetros adicionales, sin embargo puedes preservar el original query string o incluso apuntar a un query en específico.

```blade
{{ $users->appends(['sort' => 'votes'])->links() }}

{{ $users->withQueryString()->links() }}

{{ $users->fragment('foo')->links() }}
```

### Repeatable Callback Functions

Si tienes una función de llamada y necesitas re-usar multiples veces, puedes asignarlas a una variable.

```php
$userCondition = function ($query) {
    $query->where('user_id', auth()->id());
};

// Obtener los articulos que tienen comentarios desde el usuario
// y retorna solo esos comentarios del usuario.
$articles = Article::with(['comments' => $userCondition])
    ->whereHas('comments', $userCondition)
    ->get();
```

### Request: has any

Puedes revisar no solo un parámetro con  el método `$request->has()`, pero también revisar por multiples parámetros presentes con:  `$request->hasAny():

```php
public function store(Request $request)
{
    if ($request->hasAny(['api_key', 'token'])) {
        echo 'We have API key passed';
    } else {
        echo 'No authorization parameter';
    }
}
```

### Simple Pagination

En la paginación, si quieres solo mostrar los links de "Anterior / siguiente" en vez de todos los números de las páginas, solo cambia  `paginate()` para `simplePaginate()`:

```php
// En vez de
$users = User::paginate(10);

// Puedes hacer lo siguiente
$users = User::simplePaginate(10);
```

### Blade directive to add true/false conditions


Desde Laravel 8.51: `@class` es una directiva de Blade para agregar condiciones verdaderas/falsas sobre si se debe agregar una clase de CSS.

```php
<div class="@if ($active) underline @endif">`
```

Ahora:
```php
<div @class(['underline' => $active])>
```

```php
@php
    $isActive = false;
    $hasError = true;
@endphp

<span @class([
    'p-4',
    'font-bold' => $isActive,
    'text-gray-500' => ! $isActive,
    'bg-red' => $hasError,
])></span>

<span class="p-4 text-gray-500 bg-red"></span>
```

⭐ Aportación de [@Teacoders](https://twitter.com/Teacoders/status/1445417511546023938)

### Jobs can be used without queues

Jobs son bastante discutidos en la sección de  "Queues"  en la documentación oficial, pero puedes usar Jobs sin queues, solo como clases para delegar tareas.

Solo llama ``$this->dispatchNow() desde los controladores.

```php
public function approve(Article $article)
{
    //
    $this->dispatchNow(new ApproveArticle($article));
    //
}
```

### Use faker outside factories or seeders

SI quieres generar fake data, puedes usar Faker incluso fuera de factories o seeders en cualquier clase:

Ten en cuenta: para usarla en modo producción, necesitas mover el paquete faker desde `"require-dev"` a  `"require"` en el `composer.json`

```php
use Faker;

class WhateverController extends Controller
{
    public function whatever_method()
    {
        $faker = Faker\Factory::create();
        $address = $faker->streetAddress;
    }
}
```

### Schedule things

Puedes programar cosas para lanzarlas diariamente o cada cierta hora en para todo lo que necesites automatizar. Por ejemplo: un comando artisan, una clase Job, una clase invokable, una función de retorno  e incluso un script de terminal.

```php
use App\Jobs\Heartbeat;

$schedule->job(new Heartbeat)->everyFiveMinutes();
```

```php
$schedule->exec('node /home/forge/script.js')->daily();
```

```php
use App\Console\Commands\SendEmailsCommand;

$schedule->command('emails:send Taylor --force')->daily();

$schedule->command(SendEmailsCommand::class, ['Taylor', '--force'])->daily();
```

```php
protected function schedule(Schedule $schedule)
{
    $schedule->call(function () {
        DB::table('recent_users')->delete();
    })->daily();
}
```

### Search Laravel docs

Si quieres buscar en Laravel Docs por alguna palabra clave, por defecto solo te lanza el top 5 resultados. SI necesitas más resultados puedes irte directamente a el repositorio de [Github](https://github.com/laravel/docs)

### Filter route:list

Ahora en Laravel 8.34 el comando `php artisan route:list` tiene un adicional parámetro  `--except-path` para filtrar todas las rutas si no quieres mirar todas. [Original publicación](https://githurb.com/laravel/framework/pull/36619)

### Blade directive for not repeating yourself

Si quieres mantener el mismo formato de datos en los archivos blade, puedes crear tus propias directivas Blade.

En este ejemplo usamos la cantidad de dinero formateado usando el método perteneciente a Laravel Cashier.
```php
"require": {
        "laravel/cashier": "^12.9",
}
```

```php
public function boot()
{
    Blade::directive('money', function ($expression) {
        return "<?php echo Laravel\Cashier\Cashier::formatAmount($expression, config('cashier.currency')); ?>";
    });
}
```

```php
<div>Price: @money($book->price)</div>
@if($book->discount_price)
    <div>Discounted price: @money($book->dicount_price)</div>
@endif
```

### Artisan commands help

Si no estas seguro sobre qué parámetros sobre cualquier comando, o si quieres saber qué parámetros están disponibles, solo coloca:  `php artisan help [nombre del comando]`.

### Disable lazy loading when running your tests


Si quieres prevenir lazy loading cuando corremos nuestros test puedes deshabilitarlos.
```php
Model::preventLazyLoading(!$this->app->isProduction() && !$this->app->runningUnitTests());
```

⭐ aportación de [@djgeisi](https://twitter.com/djgeisi/status/1435538167290073090)

### Using two amazing helpers in Laravel will bring magic results

Usar dos helpers bastante prácticos  en Laravel te ayudará bastante...
En este caso, el servicio será llamado y re-intentado, Si sigue fallando será reportado, pero tu petición no fallará

```php
rescue(function () {
    retry(5, function () {
        $this->service->callSomething();
    }, 200);
});
```

⭐ Aportación de   [@JuanDMeGon](https://twitter.com/JuanDMeGon/status/1435466660467683328)

### Request parameter default value


Aquí  verificamos si el parámetro per_page valor, en caso de que no haya 
```php
// En vez de esto
$perPage = request()->per_page ? request()->per_page : 20;

// haz esto
$perPage = request('per_page', 20);
```

⭐ Aportación de [@devThaer](https://twitter.com/devThaer/status/1437521022631165957)

### Pass middleware directly into the route without register it

En Laravel somos capaces de colocar directamente un middleware sin registrarlo globalmente.
```php
Route::get('posts', PostController::class)
    ->middleware(['auth', CustomMiddleware::class])
```


⭐ Aportación de  [@sky_0xs](https://twitter.com/sky_0xs/status/1438258486815690766)

### Transforming an array to CssClasses

```php
use Illuminate\Support\Arr;

$array = ['p-4', 'font-bold' => $isActive, 'bg-red' => $hasError];

$isActive = false;
$hasError = true;

$classes = Arr::toCssClasses($array);

/*
 * 'p-4 bg-red'
 */
```

⭐ Aportación de  [@dietsedev](https://twitter.com/dietsedev/status/1438550428833271808)

### "upcomingInvoice" method in Laravel Cashier (Stripe)

Puedes mostrar cuanto dinero un cliente va a pagar en su siguiente ciclo de pago (billing cycle). Hay un método en Laravel Cashier para obtener el siguiente detalle de recibo de pago.
	
```php
Route::get('/profile/invoices', function (Request $request) {
    return view('/profile/invoices', [
        'upcomingInvoice' => $request->user()->upcomingInvoice(),
        'invoices' => $request-user()->invoices(),
    ]);
});
```


⭐ Aportación de [@oliverds\_](https://twitter.com/oliverds_/status/1439997820228890626)

### Laravel Request exists() vs has()

```php
// https://example.com?popular
$request->exists('popular') // true
$request->has('popular') // false

// https://example.com?popular=foo
$request->exists('popular') // true
$request->has('popular') // true
```

⭐ Aportación de [@coderahuljat](https://twitter.com/coderahuljat/status/1442191143244951552)

### There are multiple ways to return a view with variables

Existen muchas formas de retornar variables a las vistas Blade.

```php
// First way ->with()
// Primera manera 
return view('index')
    ->with('projects', $projects)
    ->with('tasks', $tasks)

// Segunda  manera
return view('index', [
        'projects' => $projects,
        'tasks' => $tasks
    ]);

// Tercera manera
$data = [
    'projects' => $projects,
    'tasks' => $tasks
];
return view('index', $data);

// Cuarta manera
return view('index', compact('projects', 'tasks'));
```

### Schedule regular shell commands

We can schedule regular shell commands within Laravel scheduled command

También podemos programar comándos de shell dentro de Laravel.

```php
// app/Console/Kernel.php

class Kernel extends ConsoleKernel
{
    protected function schedule(Schedule $schedule)
    {
        $schedule->exec('node /home/forge/script.js')->daily();
    }
}
```

⭐ Aportación de  [@anwar_nairi](https://twitter.com/anwar_nairi/status/1448985254794915845)

### HTTP client request without verifying

A veces queremos enviar peticiones HTTP saltándonos la verificación SSL en nuestro entorno local, para esto usamos: 

```php
return Http::withoutVerifying()->post('https://example.com');
```


Si queremos hacer peticiones más complejas con multiples parámetros podemos usar:

```php
return Http::withOptions([
    'verify' => false,
    'allow_redirects' => true
])->post('https://example.com');
```

⭐  Aportación de [@raditzfarhan](https://github.com/raditzfarhan)

### Test that doesn't assert anything

Existen test  que no afirma nada, simplemente lanza algo que puede o no generar una excepción

```php
class MigrationsTest extends TestCase
{
    public function test_successful_foreign_key_in_migrations()
    {
        // Lanzamos el test si las migraciones son exitosas o lanza una exepción
        $this->expectNotToPerformAssertions();


       Artisan::call('migrate:fresh', ['--path' => '/database/migrations/task1']);
    }
}
```

### "Str::mask()" method

Laravel 8.69 ha lanzado el método `Str::mask()` el cual añade al original string un carácter x numero de veces. 

```php
class PasswordResetLinkController extends Controller
{
    public function sendResetLinkResponse(Request $request)
    {
        $userEmail = User::where('email', $request->email)->value('email'); // username@domain.com

        $maskedEmail = Str::mask($userEmail, '*', 4); // user***************

        // Si lo necesitas, puedes colocar un número negativo como tercer arumento a el método
        // Lo cual indicará al método que comience a colocar el string  a partir de la distancia dada desde el final de la cadena.

        $maskedEmail = Str::mask($userEmail, '*', -16, 6); // use******domain.com
    }
}
```

⭐  Aportación de [@Teacoders](https://twitter.com/Teacoders/status/1457029765634744322)

### Extending Laravel classes


Existe un método llamado macro en muchas de las clases de Laravel. Por ejemplo en Collection, Str, Arr, Request, Cache, File y más.

Puedes definir tus propios método en sobre esas clases así:

```php
Str::macro('lowerSnake', function (string $str) {
    return Str::lower(Str::snake($str));
});

// Will return: "my-string"
Str::lowerSnake('MyString');
```

⭐  Aportación de [@mmartin_joo](https://twitter.com/mmartin_joo/status/1457635252466298885)

### Can feature

Si estás corriendo la versión `v8.70` puedes añadir el método `can()` directamente en vez de declararlo así: `middleware('can:..')`

```php
// En vez de
Route::get('users/{user}/edit', function (User $user) {
    ...
})->middleware('can:edit,user');

// Puedes realizar esto
Route::get('users/{user}/edit', function (User $user) {
    ...
})->can('edit' 'user');

// Por cierto, necesitas crear UserPolicy para poder hacer esto en ambas clases
```

⭐ Aportación de [@sky_0xs](https://twitter.com/sky_0xs/status/1458179766192853001)

### Temporary download URLs

Puedes usar  URL de descarga temporales para los archivos que contenga tu nube para prevenir acceso no permitido. Por ejemplo cuando un usuario quiere descargar un archivo, el direccionamiento a el recurso S3 su URL expira en 5 segundos.

```php
public function download(File $file)
{
    //Inicia la descrga del archivo redireccionandolo a una URL S3 que expira en 5 segundos
    return redirect()->to(
        Storage::disk('s3')->temporaryUrl($file->name, now()->addSeconds(5))
    );
}
```

⭐ Aportación de  [@Philo01](https://twitter.com/Philo01/status/1458791323889197064)

### Dealing with deeply-nested arrays

Si tienes un complejo arreglo, puedes usar el helper  `data_get()` para retornar un valor desde un arreglo dentro de un arreglo usando ".":
```php
$data = [
  0 => ['user_id' => 1, 'created_at' => 'timestamp', 'product' => {object Product}],
  1 => ['user_id' => 2, 'created_at' => 'timestamp', 'product' => {object Product}],
  2 => etc
];


// Para obtener todos los id de productos, podemos hacer esto:
data_get($data, '*.product.id');

// Como resultado sería [1, 2, 3, 4, 5, etc...]
```


En el ejemplo de abajo, si la petición, el user o el campo name no se encuentran lanzará un error  

```php
$value = $payload['request']['user']['name'];

// La función acepta valores por defecto, lo cual será retornado si no encuentra las keys
$value = data_get($payload, 'request.user.name', 'John')
```

⭐ Aportación de [@mattkingshott](https://twitter.com/mattkingshott/status/1460970984568094722)

### Customize how your exceptions are rendered

Puedes personalizar cómo las excepciones son retornados con solo añadir el método render en la excepción:

Por ejemplo, el código de abajo permite retornar JSON en vez de una vista Blade cuando la petición espera un JSON
```php
abstract class BaseException extends Exception
{
    public function render(Request $request)
    {
        if ($request->expectsJson()) {
            return response()->json([
                'meta' => [
                    'valid'   => false,
                    'status'  => static::ID,
                    'message' => $this->getMessage(),
                ],
            ], $this->getCode());
        }

        return response()->view('errors.' . $this->getCode(), ['exception' => $this], $this->getCode());
    }
}
```

```php
class LicenseExpiredException extends BaseException
{
    public const ID = 'EXPIRED';
    protected $code = 401;
    protected $message = 'Given license has expired.'
}
```

⭐Aportación de [@Philo01](https://twitter.com/Philo01/status/1461331239240192003/)

### The tap helper

El helper `tap` es una excelente manera de eliminar una declaración de retorno separada después de llamar a un método en un objeto. Hace que las cosas sean ordenadas y limpias.

```php
// Sin el método Tap
$user->update(['name' => 'John Doe']);

return $user;

// Con el método tap
return tap($user)->update(['name' => 'John Doe']);
```

⭐ Aportación de [@mattkingshott](https://twitter.com/mattkingshott/status/1462058149314183171)

### Reset all of the remaining time units


Puedes insertar un signo de exclamación dentro del método `DateTime::createFromFormat` para  eliminar las unidades de tiempo.
```php
// 2021-10-12 21:48:07.0
DateTime::createFromFormat('Y-m-d', '2021-10-12');

// 2021-10-12 00:00:00.0
DateTime::createFromFormat('!Y-m-d', '2021-10-12');

// 2021-10-12 21:00:00.0
DateTime::createFromFormat('!Y-m-d H', '2021-10-12');
```

⭐ Aportación de  [@SteveTheBauman](https://twitter.com/SteveTheBauman/status/1448045021006082054)

### Scheduled commands in the console kernel can automatically email their output if something goes wrong

¿Sabías que cualquier comando que programes en el kernel puedes automáticamente enviar el error si algo sale mal a través de email.
```php
$schedule
    ->command(PruneOrganizationsCOmmand::class)
    ->hourly()
    ->emailOutputOnFailure(config('mail.support'));
```


⭐ Aportación de  [@mattkingshott](https://twitter.com/mattkingshott/status/1463160409905455104)

### Be careful when constructing your custom filtered queries using GET parameters

Cuando construimos filtros en nuestros endpoints debemos tomar en cuenta ciertos métodos que nos pueden ayudar a evitar errores.

```php
if (request()->has('since')) {
    // example.org/?since=
    // Falla porque el valor no existe
    $query->whereDate('created_at', '<=', request('since'));
}

if (request()->input('name')) {
    // example.org/?name=0
    // Falla al aplicar el filtro porque evalua en falso
    $query->where('name', request('name'));
}

if (request()->filled('key')) {
    // Manera correcta para revisar si el parametro tiene un valor
}
```

⭐ Aportación de [@mc0de](https://twitter.com/mc0de/status/1465209203472146434)

### Dust out your bloated route file

Desempolva tu archivo de rutas  y divídelo para mantener las cosas organizadas.
```php
class RouteServiceProvider extends ServiceProvider
{
    public function boot()
    {
        $this->routes(function () {
            Route::prefix('api/v1')
                ->middleware('api')
                ->namespace($this->namespace)
                ->group(base_path('routes/api.php'));

            Route::prefix('webhooks')
                ->namespace($this->namespace)
                ->group(base_path('routes/webhooks.php'));

            Route::middleware('web')
                ->namespace($this->namespace)
                ->group(base_path('routes/web.php'));

            if ($this->app->environment('local')) {
                Route::middleware('web')
                    ->namespace($this->namespace)
                    ->group(base_path('routes/local.php'));
            }
        });
    }
}
```

⭐ Aportación de [@Philo01](https://twitter.com/Philo01/status/1466068376330153984)
### You can send e-mails to a custom log file

En Laravel puedes enviar emails a los archivos logs.

Solo tienes que colocar tu entorno de variables así: 

```
MAIL_MAILER=log
MAIL_LOG_CHANNEL=mail
```

  
Y también configurar la ruta:

```php
'mail' => [
    'driver' => 'single',
    'path' => storage_path('logs/mails.log'),
    'level' => env('LOG_LEVEL', 'debug'),
],
```


Ahora ya tienes todos tus emails en el archivo  /logs/mails.log. Es un buen caso de uso para revisar tus emails sin configurar un servicio.

⭐ Aportación de [@mmartin_joo](https://twitter.com/mmartin_joo/status/1466362508571131904)

### Markdown made easy

Laravel provee una interfaz para convertir markdown en HTML, sin necesidad de instalar nuevos paquetes.

```php
$html = Str::markdown('# Changelogfy')
```

Salida:

```
<h1>Changelogfy</h1>
```

⭐ Aportación de  [@paulocastellano](https://twitter.com/paulocastellano/status/1467478502400315394)

### Simplify if on a request with whenFilled() helper

Usualmente escribimos sentencias is si un valor está presente en la petición o no.

Podemos simplificar lo con el helper `whenFilled()` :`
```php
public function store(Request $request)
{
    $request->whenFilled('status', function (string $status)) {
        // Do something amazing with the status here!
        // Aqui entra cuando contiene algun valor
    }, function () {
        // This it called when status is not filled
        // Esto es llamado cuando el status no contiene nada
    });
}
```

⭐ Aportación de [@mmartin_joo](https://twitter.com/mmartin_joo/status/1467886802711293959)

### Pass arguments to middleware

Puedes pasar argumentos a tus middlewares para rutas específicas tan solo añadiendo ':' seguido del valor. Por ejemplo:  puedes aplicar diferentes métodos de autenticación basados en la ruta mediante un único middleware.
```php
Route::get('...')->middleware('auth.license');
Route::get('...')->middleware('auth.license:bearer');
Route::get('...')->middleware('auth.license:basic');
```

```php
class VerifyLicense
{
    public function  handle(Request $request, Closure $next, $type = null)
    {
        $licenseKey  = match ($type) {
            'basic'  => $request->getPassword(),
            'bearer' => $request->bearerToken(),
            default  => $request->get('key')
        };

        // Verify license and return response based on the authentication type
        
    }
}
```

⭐ Aportación de [@Philo01](https://twitter.com/Philo01/status/1471864630486179840)

### Get value from session and forget

Si necesitas tomar algún valor de Laravel Session y después eliminarlo, considera usar `session()->pull($value)`.  
```php
// Before
$path = session()->get('before-github-redirect', '/components');

session()->forget('before-github-redirect');

return redirect($path);

// After
return redirect(session()->pull('before-github-redirect', '/components'))
```

⭐ Aportación de [@jasonlbeggs](https://twitter.com/jasonlbeggs/status/1471905631619715077)

### $request->date() method

Ahora en Laravel 8.77 el método  `$request->date()` permite recuperar la fecha de la petición. 
[Link ](https://github.com/laravel/framework/pull/39945) de [@DarkGhostHunter](https://twitter.com/DarkGhostHunter)
### Use through instead of map when using pagination

When you want to map paginated data and return only a subset of the fields, use `through` rather than `map`. The `map` breaks the pagination object and changes it's identity. While, `through` works on the paginated data itself

Cuando queremos mapear los datos paginados y retornar solo un unos campos, usa `through` en vez de  `map`. . El método map rompe la paginación y cambia su identidad. Mientras que through trabaja sobre los datos paginados.
```php
// Don't: Mapping paginated data
$employees = Employee::paginate(10)->map(fn ($employee) => [
    'id' => $employee->id,
    'name' => $employee->name
])

// Do: Mapping paginated data
$employees = Employee::paginate(10)->through(fn ($employee) => [
    'id' => $employee->id,
    'name' => $employee->name
])
```

⭐ Aportación de [@bhaidar](https://twitter.com/bhaidar/status/1475073910383120399)

### Quickly add a bearer token to HTTP request


Hay un método  `withToken`  que permite adjuntar directamente el header  `Authorization`  a la petición

```php
// Booo!
Http::withHeader([
    'Authorization' => 'Bearer dQw4w9WgXcq'
])

// YES!
Http::withToken('dQw4w9WgXcq');
```

⭐ Aportación de [@p3ym4n](https://twitter.com/p3ym4n/status/1487809735663489024)

### Copy file or all files from a folder

Puedes usar el método `readStream`  y  `writeStream` para copiar archivos desde on disco a otro manteniendo el uso de memoria bajo.
```php
//Lista de todos los archivos de un folder
$files = Storage::disk('origin')->allFiles('/from-folder-name');

// Usando el método convencional get y put para moverlos.
foreach($files as $file) {
    Storage::disk('destination')->put(
        "optional-folder-name" . basename($file),
        Storage::disk('origin')->get($file)
    );
}

// Usando Streams para mantener el uso de memoria bajo
foreach ($files as $file) {
    Storage::disk('destination')->writeStream(
        "optional-folder-name" . basename($file),
        Storage::disk('origin')->readStream($file)
    );
}
```

⭐ Aportación de [@alanrezende](https://twitter.com/alanrezende/status/1488194257567498243)

### Sessions has() vs exists() vs missing()

¿Sabías sobre los métodos  `has, exists, missing` sobre Laravel Session?

```php
// El método has retorna true si el iteme está presente y no nullo
$request->session()->has('key');

// Este método retorna true si el item esta presente, aunque  el valor sea nullo
$request->session()->exists('key');

// El método missing retorna true si el item no está presente o si el item es nulo
$request->session()->missing('key');
```

⭐ Aportación de [@iamharis010](https://twitter.com/iamharis010/status/1489086240729145344)

### Test that you are passing the correct data to a view

¿Necesitas testear que estás enviando la información correcta a la vista? puedes usar el método `viewData` junto con el response. Aquí hay algunos 
ejemplos:

```php
/** @test */
public function it_has_the_correct_value()
{
    // ...
    $response = $this->get('/some-route');
    $this->assertEquals('John Doe', $response->viewData('name'));
}

/** @test */
public function it_contains_a_given_record()
{
    // ...
    $response = $this->get('/some-route');
    $this->assertTrue($response->viewData('users')->contains($userA));
}

/** @test */
public function it_returns_the_correct_amount_of_records()
{
    // ...
    $response = $this->get('/some-route');
    $this->assertCount(10, $response->viewData('users'));
}
```

⭐ Aportación de  [@JuanRangelTX](https://twitter.com/JuanRangelTX/status/1489944361580351490)

### Use Redis to track page views

Hacer un seguimiento de algo como las vistas de página con MySQL puede tener un impacto significativo en el rendimiento cuando se trata de un tráfico elevado. Redis es mucho mejor para esto. Puedes utilizar Redis y un comando programado para mantener a MySQL sincronizado en un intervalo fijo.

```php
Route::get('{project:slug', function (Project $project) {
    // En vez de $project->increment('views') w usamos Redis
    // Agrupamos las vistas por ej id del proyecto
    Redis::hincrby('project-views', $project->id, 1);
})
```

```php
// Console/Kernel.php
$schedule->command(UpdateProjectViews::class)->daily();

// Console/Commands/UpdateProjectViews.php
// Obtenemos todas las vistas desde nuestra instancia de Redes
$views = Redis::hgetall('project-views');


/*
[
    (id) => (views)
    1 => 213,
    2 => 100,
    3 => 341
]
 */

// Loop through all project views
foreach ($views as $projectId => $projectViews) {
    // Incrementar las vistas del projecto en nuestra tabla MYSQL
    Project::find($projectId)->increment('views', $projectViews);
}

// Eliminar todas las vistas desde nuestra instancia de Redis
Redis::del('project-views');
```

⭐ Aportación de  [@Philo01](https://twitter.com/JackEllis/status/1491909483496411140)

### to_route() helper function

Laravel 9 provee una versión más corta de `response()->route()`, echale un vistazo al siguiente código:

```php
// Vieja manera
Route::get('redirectRoute', function() {
    return redirect()->route('home');
});

// Apartir de Laravel 9
Route::get('redirectRoute', function() {
    return to_route('home');
});

```


Este helper funciona de la misma manera como `redirect()->route('home')` pero es mucho mas conciso.

⭐ Aportación de  [@CatS0up](https://github.com/CatS0up)

### Pause a long running job when queue worker shuts down

Cuando se ejecuta un Job  largo, el Job puede detenerse por estos motivos:

- Detener el trabajador.
- Enviar la señal **SIGTERM** (**SIGINT** para Horizon).
- Presionar `CTRL + C` (Linux/Windows).

Entonces, es posible que el proceso del trabajo se corrompa mientras está realizando alguna tarea.

Al verificar con `app('queue.worker')->shouldQuit`, podemos determinar si el trabajador se está apagando. De esta manera, podemos guardar el proceso actual y volver a encolar el trabajo para que cuando el trabajador de la cola se ejecute nuevamente, pueda continuar desde donde se detuvo.

Esto es muy útil en el mundo de los contenedores (Kubernetes, Docker, etc.) donde el contenedor se destruye y se vuelve a crear en cualquier momento.

```php
<?php

namespace App\Jobs;

use App\Models\User;
use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;
use Illuminate\Support\Facades\Cache;
use Illuminate\Support\Facades\Log;

class MyLongJob implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    public $timeout = 3600;

    private const CACHE_KEY = 'user.last-process-id';

    public function handle()
    {
        $processedUserId = Cache::get(self::CACHE_KEY, 0); // Get last processed item id
        $maxId = Users::max('id');

        if ($processedUserId >= $maxId) {
            Log::info("All users have already been processed!");
            return;
        }

        while ($user = User::where('id', '>', $processedUserId)->first()) {
            Log::info("Processing User ID: {$user->id}");

            // Your long work here to process user
            // Ex. Calling Billing API, Preparing Invoice for User etc.

            $processedUserId = $user->id;
            Cache::put(self::CACHE_KEY, $processedUserId, now()->addSeconds(3600)); // Updating last processed item id

            if (app('queue.worker')->shouldQuit) {
                $this->job->release(60); // Requeue the job with delay of 60 seconds
                break;
            }
        }

        Log::info("All users have processed successfully!");
    }
}
```

⭐ Aportación de [@a-h-abid](https://github.com/a-h-abid)

### Freezing Time in Laravel Tests

En tus test de Laravel, puedes necesitar congelar el tiempo. Esto es particularmente util si estás intentado hacer pruebas basado en el tiemstamp o necesitas hacer consultas basadas en fechas o tiempo.

```php
// Para congelar el tiempo, se solia ser capaz de escribir esto al inicio de tus test:
Carbon::setTestNow(Carbon::now());
// Además puedes usar el método "travelTo"
$this->travelTo(Carbon::now());
//Puedes usar el nuevo método "freezeTime" para mantener tu código limpio:
$this->freezeTime();
```

⭐ Aportación de  [@AshAllenDesign](https://twitter.com/AshAllenDesign/status/1509115721183158272)

### New squish helper

Desde Laravel 9.7 el helper  `squish`  nos permite quitar largos espacios, es como el método `trim` pero más poderoso. 

```php
$result = Str::squish(' Hello   John,         how   are   you?    ');
// Hello John, how are you?
```

⭐ Aportación de  [@mattkingshott](https://twitter.com/mattkingshott/status/1511718052752150534)

### Specify what to do if a scheduled task fails or succeeds

Puedes especificar que hacer si una tarea falla o es exitosa.

```php
$schedule->command('emails:send')
        ->daily()
        ->onSuccess(function () {
            // The task succeeded
        })
        ->onFailure(function () {
            // The task failed
        });
```

⭐ Aportación de  [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1513221529072414720)

### Scheduled command on specific environments

Para correr Laravel commandos en entornos específicos:

```php
// Not good
if (app()->environment('production', 'staging')) {
    $schedule->command('emails:send')
        ->daily();
}
// Better
$schedule->command('emails:send')
        ->daily()
        ->onEnvironment(['production', 'staging']);
```


⭐ Aportación de [@nguyenduy4994](https://twitter.com/nguyenduy4994/status/1516960273000587265)

### Add conditionable behavior to your own classes

Puedes usar el [Conditionable Trait](https://laravel.com/api/9.x/Illuminate/Support/Traits/Conditionable.html)  para evitar usar  `if/else` y promover el uso de métodos.

```php
<?php

namespace App\Services;

use Illuminate\Support\Traits\Conditionable;

class MyService
{
    use Conditionable;

    // ...
}
```

```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use App\Http\Requests\MyRequest;
use App\Services\MyService;

class MyController extends Controller
{
    public function __invoke(MyRequest $request, MyService $service)
    {
        // Not goodsi 
        $service->addParam($request->param);

        if ($request->has('something')) {
            $service->methodToBeCalled();
        }

        $service->execute();
        // ---

        // Better
        $service->addParam($request->param)
            ->when($request->has('something'), fn ($service) => $service->methodToBeCalled())
            ->execute();
        // ---

        // ...
    }
}
```

### Perform Action when Job has failed

En algunos casos, queremos realizar alguna acción cuando un Job ha fallado. Por ejemplo, enviar una notificación a través de email.

Para este propósito, podemos usar el método  `failed()`  en la clase Job sobre el método `handle()`: 

```php
namespace App\Jobs\Invoice;
use Illuminate\Bus\Batchable;
use Illuminate\Bus\Queueable;
use Illuminate\Queue\SerializesModels;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Throwable;

class CalculateSingleConsignment implements ShouldQueue
{
    use Batchable, Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    // ... __construct() method, handle() method, etc.

    public function failed(Throwable $exception)
    {
        // Perform any action here when job has failed
     }
}
```

⭐ Aportación de  [@pauloimon](https://github.com/pauloimon)

### Ignore Database when Job has failed

Si alguna vez necesitas evitar el uso de la base de datos cuando un trabajo falla, puedes hacer una de las siguientes cosas para omitir la base de datos:

- Establecer la variable de entorno `QUEUE_FAILED_DRIVER` con el valor `null`. Funciona a partir de Laravel 8 y versiones superiores.
- Establecer el valor de `failed` en `null` en el archivo `config/queue.php`, reemplazando el array (como en el código a continuación). Esto funciona para Laravel 7 y versiones anteriores.
```php
    'failed' => null,
```
  
¿Por qué querrías esto? Para aplicaciones en las que no es necesario almacenar trabajos fallidos y necesitan tener un alto TPS (transacciones por segundo), omitir la base de datos puede ser muy favorable, ya que no estamos accediendo a la base de datos, lo que ahorra tiempo y evita que la base de datos se caiga.

⭐ Aportación de[@a-h-abid](https://github.com/a-h-abid)
