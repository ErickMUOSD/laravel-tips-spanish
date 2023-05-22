## Log and debug

⬆️ [Menú principal](README.md#laravel-tips) ⬅️ [Anterior (Factories)](factories.md) ➡️ [Siguiente (API)](api.md)

- [Logging with parameters](#logging-with-parameters)
- [Log Long Running Laravel Queries](#log-long-running-laravel-queries)
- [Benchmark class](#benchmark-class)
- [More convenient DD](#more-convenient-dd)
- [Log with context](#log-with-context)
- [Quickly output an Eloquent query in its SQL form](#quickly-output-an-eloquent-query-in-its-sql-form)
- [Log all the database queries during development](#log-all-the-database-queries-during-development)
- [Discover all events fired in one request](#discover-all-events-fired-in-one-request)

### Logging with parameters

Puedes colocar  `Lg::info()` o el helper `info()`  el mensaje con parametros adicionales, para tener más contexto de lo que sucedió. 

```php
Log::info('User failed to login.', ['id' => $user->id]);
```

### Log Long Running Laravel Queries

Cuando tenemos nuestra aplicación en producción , podemos registrar  consultas que están tardando con el objetivo de debuguear más rápido.

```php
DB::enableQueryLog();

DB::whenQueryingForLongerThen(1000, function ($connection) {
     Log::warning(
          'Long running queries have been detected.',
          $connection->getQueryLog()
     );
});
```

⭐Aportación de [@realstevebauman](https://twitter.com/realstevebauman/status/1576980397552185344)

### Benchmark class

En Laravel 9.32 tenemos una clase Benchmark que puede medir el tiemo de duración de cada tarea.

```php
class OrderController
{
     public function index()
     {
          return Benchmark::measure(fn () => Order::all()),
     }
}
```

⭐Aportación de [@mmartin_joo](https://twitter.com/mmartin_joo/status/1583096196494553088)

### More convenient DD

En vez de imprimir el resultado  `dd($result)` podemos colocar `->dd()` como método directamente al final de nuestras sentencias Eloquent y Colleciones.

```php
// En vez de esto
$users = User::where('name', 'Taylor')->get();
dd($users);
// Haz esto
$users = User::where('name', 'Taylor')->get()->dd();
```

### Log with context

Apartir de Laravel 8.49 `Log::withContext()`  puede ayudarte a diferenciar los Logs entre direferentes peticiones.

Puedes crear un Middleware y colocar su contexto, entonces todos los Log Messages contendrán ese contexto, y serás capaz de buscarlos mucho más fácil a la hora de debugear.
```php
public function handle(Request $request, Closure $next)
{
    $requestId = (string) Str::uuid();

    Log::withContext(['request-id' => $requestId]);

    $response = $next($request);

    $response->header('request-id', $requestId);

    return $response;
}
```

### Quickly output an Eloquent query in its SQL form

Si quieres imprimir la consulta que será ejecutada en formato SQL de nuestros modelos Eloquent  podemos invocar el método **toSql()**.
```php
$invoices = Invoice::where('client', 'James pay')->toSql();

dd($invoices)
// select * from `invoices` where `client` = ?
```

⭐ Aportación de  [@devThaer](https://twitter.com/devThaer/status/1438816135881822210)

### Log all the database queries during development

Si queremos hacer Logs de todas las consultas a la base de datos durante el desarollo, añade este snippet en el AppServiceProvider:

```php
public function boot()
{
    if (App::environment('local')) {
        DB::listen(function ($query) {
            logger(Str::replaceArray('?', $query->bindings, $query->sql));
        });
    }
}
```

⭐ Aportación de  [@mmartin_joo](https://twitter.com/mmartin_joo/status/1473262634405449730) 

### Discover all events fired in one request

SI necesitas implementar un nuevo listener a un evento específico pero no conoces su nombre, puedes hacer un Log a todos los eventos lanzados durante la petición.

Puedes usar el método  `\Illuminate\Support\Facades\Event::listen()`  en la función `boot()` de   `app/Providers/EventServiceProvider.php`  para recibir todos los eventos lazandos.

**Importante:** si usas el `Log` facade dentro de este listener evento entonces necesitarás excluir eventos llamados  `Illuminate\Log\Events\MessageLogged` para evitar ciclos infinitos, por ejemplo : `if ($event == 'Illuminate\\Log\\Events\\MessageLogged') return;`.
```php
// Incluir evento
use Illuminate\Support\Facades\Event;

// En la clase  EventServiceProvider
public function boot()
{
    parent::boot();

    Event::listen('*', function ($event, array $data) {
        // Log
        error_log($event);

        // Log la información del evento delegada a los parametros.
        error_log(json_encode($data, JSON_PRETTY_PRINT));
    });
}
```

⭐ Aportación de [@MuriloChianfa](https://github.com/MuriloChianfa)