## API

⬆️ [Menú](README.md#laravel-tips) ⬅️ [Anterior (Log and debug)](log-and-debug.md) ➡️ [Siguiente (Other)](other.md)

- [API Resources: With or Without "data"?](#api-resources-with-or-without-data)
- [Conditional Relationship Counts on API Resources](#conditional-relationship-counts-on-api-resources)
- [API Return "Everything went ok"](#api-return-everything-went-ok)
- [Avoid N+1 queries in API resources](#avoid-n1-queries-in-api-resources)
- [Get Bearer Token from Authorization header](#get-bearer-token-from-authorization-header)
- [Sorting Your API Results](#sorting-your-api-results)
- [Customize Exception Handler For API](#customize-exception-handler-for-api)
- [Force JSON Response For API Requests](#force-json-response-for-api-requests)
- [API Versioning](#api-versioning)

### API Resources: With or Without "data"?

Si usas Eloquent API Resources para retonar el resultado, este será envuelto en un 'data'. SI quieres quitarlo solo añade `JsonResource::withoutWrapping();` en `app/Providers/AppServiceProvider.php`.

```php
class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        JsonResource::withoutWrapping();
    }
}
```

⭐ Aportación de   [@phillipmwaniki](https://twitter.com/phillipmwaniki/status/1445230637544321029)

### Conditional Relationship Counts on API Resources

También puedes condicionalmente incluir los modelos contados de la relación en el resource response usando el método whenCounted. Al realizar esto el si atributo `model_count`   no es incluido no será puesto en el resource. 
```php
public function toArray($request)
{
     return [
          'id' => $this->id,
          'name' => $this->name,
          'email' => $this->email,
          'posts_count' => $this->whenCounted('posts'),
          'created_at' => $this->created_at,
          'updated_at' => $this->updated_at,
     ];
}
```

⭐ Aportación de  [@mvpopuk](https://twitter.com/mvpopuk/status/1570480977507504128)

### API Return "Everything went ok"

Si en tú API tienes un endpoint que  el cual realiza algunas operaciones pero no retorna una respuesta y si solo quieres retornar algo como "everything ok", entonces solo debes retornar el status 204 "No content" pues con Laravel es fácil usando: `return response()->noContent();`.
```php
public function reorder(Request $request)
{
    foreach ($request->input('rows', []) as $row) {
        Country::find($row['id'])->update(['position' => $row['position']]);
    }

    return response()->noContent();
}
```

### Avoid N+1 queries in API resources

Para evitar el famoso problema N´1 queries en API resources utiliza  el método `whenLoaded()` .
Esto solo añadirá la relación Department  si es que ya ha sido cargada en el modelo Employee.
Sin el método  `whenLoaded()` siempre relaizará una consulta extra para el modelo Department.

```php
class EmployeeResource extends JsonResource
{
    public function toArray($request): array
    {
        return [
            'id' => $this->uuid,
            'fullName' => $this->full_name,
            'email' => $this->email,
            'jobTitle' => $this->job_title,
            'department' => DepartmentResource::make($this->whenLoaded('department')),
        ];
    }
}
```

⭐ Aportación de [@mmartin_joo](https://twitter.com/mmartin_joo/status/1473987501501071362) 

### Get Bearer Token from Authorization header

La función  `bearerToken()`  is bastante práctica cuando estamos trabajando con Apis y necesitamos obtener el token del Authorization  header.

```php

//No necesitas parsear los headers de la API manualmente:
$tokenWithBearer = $request->header('Authorization');
$token = substr($tokenWithBearer, 7);

// Haz esto en vez
$token = $request->bearerToken();
```

⭐ Aportación de [@iamharis010](https://twitter.com/iamharis010/status/1488413755826327553)

### Sorting Your API Results

Single-column API sorting, with direction control
Ordenación de API de una sola columna, con control de dirección.

```php
// Parametros /dogs?sort=name and /dogs?sort=-name
Route::get('dogs', function (Request $request) {
  
    // Obtén el parámetro de consulta de ordenación (o usa la ordenación predeterminada "name")
    $sortColumn = $request->input('sort', 'name');

    // Establece la dirección de ordenación en función de si la clave comienza con - // utilizando la función auxiliar Str::startsWith() de Laravel
    $sortDirection = Str::startsWith($sortColumn, '-') ? 'desc' : 'asc';
    $sortColumn = ltrim($sortColumn, '-');

    return Dog::orderBy($sortColumn, $sortDirection)
        ->paginate(20);
});
```

Realizamos lo mismo con multiples columnas   (e.g., ?sort=name,-weight).
```php
// Handles ?sort=name,-weight
Route::get('dogs', function (Request $request) {
    // Grab the query parameter and turn it into an array exploded by ,
    $sorts = explode(',', $request->input('sort', ''));

    // Create a query
    $query = Dog::query();

    // Add the sorts one by one
    foreach ($sorts as $sortColumn) {
        $sortDirection = Str::startsWith($sortColumn, '-') ? 'desc' : 'asc';
        $sortColumn = ltrim($sortColumn, '-');

        $query->orderBy($sortColumn, $sortDirection);
    }

    // Return
    return $query->paginate(20);
});
```
---

### Customize Exception Handler For API

#### Laravel 8 and below:

Para poder personalizar las excepciones lanzadas por Laravel (not found, not authorized) existe un método `render()` in la clase `App\Exceptions`:
```php
   public function render($request, Exception $exception)
    {
        if ($request->wantsJson() || $request->is('api/*')) {
            if ($exception instanceof ModelNotFoundException) {
                return response()->json(['message' => 'Item Not Found'], 404);
            }

            if ($exception instanceof AuthenticationException) {
                return response()->json(['message' => 'unAuthenticated'], 401);
            }

            if ($exception instanceof ValidationException) {
                return response()->json(['message' => 'UnprocessableEntity', 'errors' => []], 422);
            }

            if ($exception instanceof NotFoundHttpException) {
                return response()->json(['message' => 'The requested link does not exist'], 400);
            }
        }

        return parent::render($request, $exception);
    }
```

#### Laravel 9 and above:

Para poder personalizar las excepciones lanzadas por Laravel (not found, not authorized) existe un método `render()` in la clase `App\Exceptions`:
```php
    public function register()
    {
        $this->renderable(function (ModelNotFoundException $e, $request) {
            if ($request->wantsJson() || $request->is('api/*')) {
                return response()->json(['message' => 'Item Not Found'], 404);
            }
        });

        $this->renderable(function (AuthenticationException $e, $request) {
            if ($request->wantsJson() || $request->is('api/*')) {
                return response()->json(['message' => 'unAuthenticated'], 401);
            }
        });
        $this->renderable(function (ValidationException $e, $request) {
            if ($request->wantsJson() || $request->is('api/*')) {
                return response()->json(['message' => 'UnprocessableEntity', 'errors' => []], 422);
            }
        });
        $this->renderable(function (NotFoundHttpException $e, $request) {
            if ($request->wantsJson() || $request->is('api/*')) {
                return response()->json(['message' => 'The requested link does not exist'], 400);
            }
        });
    }
```

⭐ Aportación de [Feras Elsharif](https://github.com/ferasbbm).

---

### Force JSON Response For API Requests

Si tienes una API construida y te encuentras con un error cuando la petición no contiene el header  "Accept: application/JSON " entonces el error debe ser retornado como HTML, para evitar esto podemos forzar todas las respuestas en JSON

El primer paso es crear un middleware con este comando:
```console
php artisan make:middleware ForceJsonResponse
```

Escribe este código en la archivo dentro de la  función handle()  `App/Http/Middleware/ForceJsonResponse.php` :

```php
public function handle($request, Closure $next)
{
    $request->headers->set('Accept', 'application/json');
    return $next($request);
}
```

Segundo, registra el middleware creado previamente en  el archivo app/Http/Kernel.php:

```php
protected $middlewareGroups = [        
    'api' => [
        \App\Http\Middleware\ForceJsonResponse::class,
    ],
];
```

⭐ Aportación de [Feras Elsharif](https://github.com/ferasbbm)

---

### API Versioning

#### When to version?


Si estás trabajando en un proyecto que podría tener múltiples versiones en el futuro o tus endpoints tienen cambios que rompen la compatibilidad, como un cambio en el formato de los datos de respuesta y quieres asegurarte de que la versión de la API siga funciona cuando se hayan hecho cambiós en el códgio.

#### Change The Default Route Files 

El primer paso es cambiar el trazado de rutas en el archivo  `App\Providers\RouteServiceProvider`, así que comencemos:

#### Laravel 8 and above:

Añade la propiedad 'ApiNamespace' 
```php
/**
 * @var string
 *
 */
protected string $ApiNamespace = 'App\Http\Controllers\Api';
```

Dentro del método boot añade el siguiente código:

```php
$this->routes(function () {
     Route::prefix('api/v1')
        ->middleware('api')
        ->namespace($this->ApiNamespace.'\\V1')
        ->group(base_path('routes/API/v1.php'));
        }
    
    //for v2
     Route::prefix('api/v2')
            ->middleware('api')
            ->namespace($this->ApiNamespace.'\\V2')
            ->group(base_path('routes/API/v2.php'));
});
```


#### Laravel 7 and below:

Añade la propiedad 'ApiNamespace' 

```php
/**
 * @var string
 *
 */
protected string $ApiNamespace = 'App\Http\Controllers\Api';
```

Dentro del método map, añade el siguiente código

```php
// remove this $this->mapApiRoutes(); 
    $this->mapApiV1Routes();
    $this->mapApiV2Routes();
```

y añade estos métodos:

```php
  protected function mapApiV1Routes()
    {
        Route::prefix('api/v1')
            ->middleware('api')
            ->namespace($this->ApiNamespace.'\\V1')
            ->group(base_path('routes/Api/v1.php'));
    }

  protected function mapApiV2Routes()
    {
        Route::prefix('api/v2')
            ->middleware('api')
            ->namespace($this->ApiNamespace.'\\V2')
            ->group(base_path('routes/Api/v2.php'));
    }
```

#### Controller Folder Versioning

```
Controllers
└── Api
    ├── V1
    │   └──AuthController.php
    └── V2
        └──AuthController.php
```

#### Route File Versioning

```
routes
└── Api
   │    └── v1.php     
   │    └── v2.php 
   └── web.php
```

⭐ Aportación de  [Feras Elsharif](https://github.com/ferasbbm)