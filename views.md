## Views

⬆️ [Menú principal](README.md#laravel-tips) ⬅️ [Anterior (Migrations)](migrations.md) ➡️ [Siguiente (Routing)](routing.md)

- [$loop variable in foreach](#loop-variable-in-foreach)
- [You can use Blade to generate more than HTML](#you-can-use-blade-to-generate-more-than-html)
- [Short attribute syntax for Blade Components](#short-attribute-syntax-for-blade-components)
- [Share one variable with multiple views](#share-one-variable-with-multiple-views)
- [Does view file exist?](#does-view-file-exist)
- [Error code Blade pages](#error-code-blade-pages)
- [View without controllers](#view-without-controllers)
- [Blade @auth](#blade-auth)
- [Two-level $loop variable in Blade](#two-level-loop-variable-in-blade)
- [Create Your Own Blade Directive](#create-your-own-blade-directive)
- [Blade Directives: IncludeIf, IncludeWhen, IncludeFirst](#blade-directives-includeif-includewhen-includefirst)
- [Use Laravel Blade-X variable binding to save even more space](#use-laravel-blade-x-variable-binding-to-save-even-more-space)
- [Blade components props](#blade-components-props)
- [Blade Autocomplete typehint](#blade-autocomplete-typehint)
- [Component Syntax Tip](#component-syntax-tip)
- [Automatically highlight nav links](#automatically-highlight-nav-links)
- [Cleanup loops](#cleanup-loops)
- [Simple way to tidy up your Blade views](#simple-way-to-tidy-up-your-blade-views)
- [Checked blade directive](#checked-blade-directive)
- [Selected blade directive](#selected-blade-directive)

### $loop variable in foreach

Dentro de cada foreach, puedes revisar if la actual iteración es la primera o la ultima usando la variable `$loop`

```blade
@foreach ($users as $user)
     @if ($loop->first)
        Esta es la primera iteración.
     @endif

     @if ($loop->last)
        Esta es la ultima iteración.
     @endif

     <p>This is user {{ $user->id }}</p>
@endforeach
```

Existen otras propiedades como `$loop->iteration` or `$loop->count`.

Consulta mas en la [Documentatión oficial](https://laravel.com/docs/master/blade#the-loop-variable).

### You can use Blade to generate more than HTML

Puedes usar las vistas  Blade para generar  cualquier string dinámico o archivo que deseés. Por ejemplo un script o un archivo sitemap .

Solo necesitas llamar al método `render()` en una vista para traer el resultado como formato string.

```php
$script = view('deploy-script')->render();

$ssh = $this->createSshConnection();

info("Executing deploy script...");
$process = $ssh->execute(explode("\n", $script));
```

⭐ Aportación de [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1566620670888275968/)

### Short attribute syntax for Blade Components

Disponible apartir de  Laravel 9.32.

Actual sintaxis:

```blade
<x-profile :user-id="$userId"></x-profile>
```

Sintaxis corta:

```blade
<x-profile :$userId></x-profile>
```

### Share one variable with multiple views

¿Alguna vez necesitaste compartir una variable sobre multiples vistas en Laravel? Aquí hay una simple solución para eso. 

```php
use App\Models\Post;
use Illuminate\Support\Facades\View;
use Illuminate\Support\Facades\Schema;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        if (Schema::hasTable('posts')) {
            View::share('recentPosts', Post::latest()->take(3)->get());
        }
    }
}
```

⭐ Aportación de  [@codewithdary](https://twitter.com/codewithdary)

### Does view file exist?

Puedes revisar si una vista existe antes de rendereala.

```php
if (view()->exists('custom.page')) {
 // Load the view
}
```

Incluso puedes renderear multiples vistas y solo la primera que exista será rendereada.

```php
return view()->first(['custom.dashboard', 'dashboard'], $data);
```

### Error code Blade pages

¿Te ha pasado que en tu aplicación al momento de mostrar un erro HTTP como 404 carga una vista 0 agradable para la experiencia del usuario? Laravel nos permite personalizar esas vistas para cada uno de los errores HTTP, para esto debes crear el archivo en la ruta  `resources/views/errors/404.blade.php` y con el nombre del error.  De esta manera rendereara el contenido cuando se presente ese error.

### View without controllers

Si necesitas mostrar una vista blade especifica, no es necesario crear el método en un nuevo controlador, en vez de eso utiliza `Route::view()`.

```php
// En vez de esto
Route::get('about', 'TextsController@about');
// Y esto
class TextsController extends Controller
{
    public function about()
    {
        return view('texts.about');
    }
}
// Haz esto
Route::view('about', 'texts.about');
```

### Blade @auth

En vez de usar una estructura condicional para verificar  si el usuario está logueado, usa la directiva `@auth`.

Convencional forma:

```blade
@if(auth()->user())
    // Este usuario está autenticado
@endif
```

Forma corta:

```blade
@auth
    //  Este usuario está autenticado
@endauth
```

Lo opuesto es la directiva`@guest` 

```blade
@guest
    // Este usuario no está autenticado
@endguest
```

### Two-level $loop variable in Blade

En el bucle "foreach" de Blade, puedes utilizar la variable $loop incluso en un bucle de dos niveles para acceder a la variable del padre.

```blade
@foreach ($users as $user)
    @foreach ($user->posts as $post)
        @if ($loop->parent->first)
            This is first iteration of the parent loop.
        @endif
    @endforeach
@endforeach
```

### Create Your Own Blade Directive

Es sencillo, solo necesitas añadir tu propio método en`app/Providers/AppServiceProvider.php`. Por ejemplo, si quieres que tu nuevo método remplace todas las nuevas etiquetas `<br>` con nuevas lineas.

```blade
<textarea>@br2nl($post->post_text)</textarea>
```

Añade esta directivia al archivo AppServiceProvider  en el método `boot()`:

```php
public function boot()
{
    Blade::directive('br2nl', function ($string) {
        return "<?php echo preg_replace('/\<br(\s*)?\/?\>/i', \"\n\", $string); ?>";
    });
}
```

### Blade Directives: IncludeIf, IncludeWhen, IncludeFirst

Si no estas seguro qué archivo blade existe o no , puedes usar estas directivas de condición:

Esto solo cargará la la vista si el archivo existe.

```blade
@includeIf('partials.header')
```

Esto cargará la vista si el usuario tiene el rol 1.

```blade
@includeWhen(auth()->user()->role_id == 1, 'partials.header')
```

Esto cargará la vista adminlte.header, si no se encuentra, cargará el default.header.

```blade
@includeFirst('adminlte.header', 'default.header')
```

### Use Laravel Blade-X variable binding to save even more space

```blade
// Usando la forma convencional
@include("components.post", ["title" => $post->title])

// Usando Blade-X
<x-post link="{{ $post->title }}" />

// Usando Blade-X binding
<x-post :link="$post->title" />
```

⭐ Aportación de  [@anwar_nairi](https://twitter.com/anwar_nairi/status/1442441888787795970)

### Blade components props

```blade
// button.blade.php
@props(['rounded' => false])

<button {{ $attributes->class([
    'bg-red-100 text-red-800',
    'rounded' => $rounded
    ]) }}>
    {{ $slot }}
</button>

// view.blade.php
// Non-rounded:
<x-button>Submit</x-button>

// Rounded:
<x-button rounded>Submit</x-button>
```

⭐Aportación de [@godismyjudge95](https://twitter.com/godismyjudge95/status/1448825909167931396)

### Blade Autocomplete typehint

```blade
@php
    /* @var App\Models\User $user */
@endphp

<div>
    // your ide will typehint the property for you
    {{$user->email}}
</div>
```

⭐Aportación de [@freekmurze](https://twitter.com/freekmurze/status/1455466663927746560)

### Component Syntax Tip

¿Sabías que si usas ` :` antes del parametro  del componente, no hay necesidad de colocar el  `{}`?

```blade
<x-navbar title="{{ $title }}"/>

// Haz esto en vez

<x-navbar :title="$title"/>
```

⭐Aportación de [@sky_0xs](https://twitter.com/sky_0xs/status/1457056634363072520)

### Automatically highlight nav links

Resalta automaticamente nav links cuando la URL coincide, o pasa el nombre de una ruta.

Un componente de blade con algunas clases y helpers hace esto ridiculamente fácil para mostrar el  componente actvio o inactvio.

```php
class NavLink extends Component
{
    public function __construct($href, $active = null)
    {
        $this->href = $href;
        $this->active = $active ?? $href;
    }

    public function render(): View
    {
        $classes = ['font-medium', 'py-2', 'text-primary' => $this->isActive()];

        return view('components.nav-link', [
            'class' => Arr::toCssClasses($classes);
        ]);
    }

    protected function isActive(): bool
    {
        if (is_bool($this->active)) {
            return $this->active;
        }

        if (request()->is($this->active)) {
            return true;
        }

        if (request()->fullUrlIs($this->active)) {
            return true;
        }

        return request()->routeIs($this->active);
    }
}
```

```blade
<a href="{{ $href }}" {{ $attributes->class($class) }}>
    {{ $slot }}
</a>
```

```blade
<x-nav-link :href="route('projects.index')">Projects</x-nav-link>
<x-nav-link :href="route('projects.index')" active="projects.*">Projects</x-nav-link>
<x-nav-link :href="route('projects.index')" active="projects/*">Projects</x-nav-link>
<x-nav-link :href="route('projects.index')" :active="$tab = 'projects'">Projects</x-nav-link>
```

⭐ Aportación de [@mpskovvang](https://twitter.com/mpskovvang/status/1459646197635944455)

### Cleanup loops

¿Sabías que las directivas `@each` blade pueden ayudarte a tener un código más limpio en tus templates?

```blade
// good
@foreach($item in $items)
    <div>
        <p>Name: {{ $item->name }}
        <p>Price: {{ $item->price }}
    </div>
@endforeach

// better (HTML extracted into partial)
@each('partials.item', $items, 'item')
```

⭐ Aportación de [@kirschbaum_dev](https://twitter.com/kirschbaum_dev/status/1463205294914297861)

### Simple way to tidy up your Blade views

Aquí te presento una manera de hacer el código más limpio en tus vistas Blade.

Usa el `forelse loop`, en vez de usar el `foreach loop` convencional junto con una directiva `@if`.

```blade
<!-- if/loop combination -->
@if ($orders->count())
    @foreach($orders as $order)
        <div>
            {{ $order->id }}
        </div>
    @endforeach
@else
    <p>You haven't placed any orders yet.</p>
@endif

<!-- Forelse alternative -->
@forelse($orders as $order)
    <div>
        {{ $order->id }}
    </div>
@empty
    <p>You haven't placed any orders yet.</p>
@endforelse
```

⭐Aportación de [@alexjgarrett](https://twitter.com/alexjgarrett/status/1465674086022107137)

### Checked blade directive

En Laravel 9 trae una nueva característica en forma de directiva `checked` Blade.

Esto nos permite tener un código más limpio en nuestras vistas.

```blade
// Before Laravel 9:
<input type="radio" name="active" value="1" {{ old('active', $user->active) ? 'checked' : '' }}/>
<input type="radio" name="active" value="0" {{ old('active', $user->active) ? '' : 'checked' }}/>

// Laravel 9
<input type="radio" name="active" value="1" @checked(old('active', $user->active))/>
<input type="radio" name="active" value="0" @checked(!old('active', $user->active))/>
```

⭐Aportación de [@AshAllenDesign](https://twitter.com/AshAllenDesign/status/1489567000812736513)

### Selected blade directive

En Laravel 9 trae una nueva característica en forma de directiva `selected` Blade para nuestros elementos Select de HTML.

Esto nos permite tener un código más limpio en nuestras vistas.

```blade
// Before Laravel 9:
<select name="country">
    <option value="India" {{ old('country') ?? $country == 'India' ? 'selected' : '' }}>India</option>
    <option value="Pakistan" {{ old('country') ?? $country == 'Pakistan' ? 'selected' : '' }}>Pakistan</option>
</select>

// Laravel 9
<select name="country">
    <option value="India" @selected(old('country') ?? $country == 'India')>India</option>
    <option value="Pakistan" @selected(old('country') ?? $country == 'Pakistan')>Pakistan</option>
</select>
```

⭐Aportación de [@VijayGoswami](https://vijaygoswami.in)
