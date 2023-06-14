## DB Models and Eloquent

⬆️ [Go to main menu](README.md#laravel-tips) ➡️ [Next (Models Relations)](models-relations.md)

- [Reuse or clone query()](#reuse-or-clone-query)
- [Remember to use bindings in your raw queries](#remember-to-use-bindings-in-your-raw-queries)
- [Small cheat-sheet for using Full-Text Search with Laravel on MySQL](#small-cheat-sheet-for-using-full-text-search-with-laravel-on-mysql)
- [Merging eloquent collections](#merging-eloquent-collections)
- [Perform operation without modifying updated_at field](#perform-operation-without-modifying-updated_at-field)
- [You can write transaction-aware code](#you-can-write-transaction-aware-code)
- [Eloquent scopes inside of other relationships](#eloquent-scopes-inside-of-other-relationships)
- [New `rawValue()` method since Laravel 9.37](#new-rawvalue-method-since-laravel-937)
- [Load data faster when the targeted value is an integer](#load-data-faster-when-the-targeted-value-is-an-integer)
- [Load data completed between two timestamps](#load-data-completed-between-two-timestamps)
- [Pass a raw query to order your results](#pass-a-raw-query-to-order-your-results)
- [Eloquent where date methods](#eloquent-where-date-methods)
- [Increments and decrements](#increments-and-decrements)
- [No timestamp columns](#no-timestamp-columns)
- [Soft-deletes: multiple restore](#soft-deletes-multiple-restore)
- [Model all: columns](#model-all-columns)
- [To Fail or not to Fail](#to-fail-or-not-to-fail)
- [Column name change](#column-name-change)
- [Map query results](#map-query-results)
- [Change Default Timestamp Fields](#change-default-timestamp-fields)
- [Quick Order by created_at](#quick-order-by-created_at)
- [Automatic Column Value When Creating Records](#automatic-column-value-when-creating-records)
- [DB Raw Query Calculations Run Faster](#db-raw-query-calculations-run-faster)
- [More than One Scope](#more-than-one-scope)
- [No Need to Convert Carbon](#no-need-to-convert-carbon)
- [Grouping by First Letter](#grouping-by-first-letter)
- [Never Update the Column](#never-update-the-column)
- [Find Many](#find-many)
- [Find Many and return specific columns](#find-many-and-return-specific-columns)
- [Find by Key](#find-by-key)
- [Use UUID instead of auto-increment](#use-uuid-instead-of-auto-increment)
- [Sub-selects in Laravel Way](#sub-selects-in-laravel-way)
- [Hide Some Columns](#hide-some-columns)
- [Exact DB Error](#exact-db-error)
- [Soft-Deletes with Query Builder](#soft-deletes-with-query-builder)
- [Good Old SQL Query](#good-old-sql-query)
- [Use DB Transactions](#use-db-transactions)
- [Update or Create](#update-or-create)
- [Forget Cache on Save](#forget-cache-on-save)
- [Change Format Of Created_at and Updated_at](#change-format-of-created_at-and-updated_at)
- [Storing Array Type into JSON](#storing-array-type-into-json)
- [Make a Copy of the Model](#make-a-copy-of-the-model)
- [Reduce Memory](#reduce-memory)
- [Force query without $fillable/$guarded](#force-query-without-fillableguarded)
- [3-level structure of parent-children](#3-level-structure-of-parent-children)
- [Perform any action on failure](#perform-any-action-on-failure)
- [Check if record exists or show 404](#check-if-record-exists-or-show-404)
- [Abort if condition failed](#abort-if-condition-failed)
- [Fill a column automatically while you persist data to the database](#fill-a-column-automatically-while-you-persist-data-to-the-database)
- [Extra information about the query](#extra-information-about-the-query)
- [Using the doesntExist() method in Laravel](#using-the-doesntexist-method-in-laravel)
- [Trait that you want to add to a few Models to call their boot() method automatically](#trait-that-you-want-to-add-to-a-few-models-to-call-their-boot-method-automatically)
- [There are two common ways of determining if a table is empty in Laravel](#there-are-two-common-ways-of-determining-if-a-table-is-empty-in-laravel)
- [How to prevent “property of non-object” error](#how-to-prevent-property-of-non-object-error)
- [Get original attributes after mutating an Eloquent record](#get-original-attributes-after-mutating-an-eloquent-record)
- [A simple way to seed a database](#a-simple-way-to-seed-a-database)
- [The crossJoinSub method of the query constructor](#the-crossjoinsub-method-of-the-query-constructor)
- [Belongs to Many Pivot table naming](#belongs-to-many-pivot-table-naming)
- [Order by Pivot Fields](#order-by-pivot-fields)
- [Find a single record from a database](#find-a-single-record-from-a-database)
- [Automatic records chunking](#automatic-records-chunking)
- [Updating the model without dispatching events](#updating-the-model-without-dispatching-events)
- [Periodic cleaning of models from obsolete records](#periodic-cleaning-of-models-from-obsolete-records)
- [Immutable dates and casting to them](#immutable-dates-and-casting-to-them)
- [The findOrFail method also accepts a list of ids](#the-findorfail-method-also-accepts-a-list-of-ids)
- [Prunable trait to automatically remove models from your database](#prunable-trait-to-automatically-remove-models-from-your-database)
- [withAggregate method](#withaggregate-method)
- [Date convention](#date-convention)
- [Eloquent multiple upserts](#eloquent-multiple-upserts)
- [Retrieve the Query Builder after filtering the results](#retrieve-the-query-builder-after-filtering-the-results)
- [Custom casts](#custom-casts)
- [Order based on a related model's average or count](#order-based-on-a-related-models-average-or-count)
- [Return transactions result](#return-transactions-result)
- [Remove several global scopes from query](#remove-several-global-scopes-from-query)
- [Order JSON column attribute](#order-json-column-attribute)
- [Get single column's value from the first result](#get-single-columns-value-from-the-first-result)
- [Check if altered value changed key](#check-if-altered-value-changed-key)
- [New way to define accessor and mutator](#new-way-to-define-accessor-and-mutator)
- [Another way to do accessors and mutators](#another-way-to-do-accessors-and-mutators)
- [When searching for the first record, you can perform some actions](#when-searching-for-the-first-record-you-can-perform-some-actions)
- [Directly convert created_at date to human readable format](#directly-convert-created_at-date-to-human-readable-format)
- [Ordering by an Eloquent Accessor](#ordering-by-an-eloquent-accessor)
- [Check for specific model was created or found](#check-for-specific-model-was-created-or-found)
- [Laravel Scout with database driver](#laravel-scout-with-database-driver)
- [Make use of the value method on the query builder](#make-use-of-the-value-method-on-the-query-builder)
- [Pass array to where method](#pass-array-to-where-method)
- [Return the primary keys from models collection](#return-the-primary-keys-from-models-collection)
- [Force Laravel to use eager loading](#force-laravel-to-use-eager-loading)
- [Make all your models mass assignable](#make-all-your-models-mass-assignable)
- [Hiding columns in select all statements](#hiding-columns-in-select-all-statements)
- [JSON Where Clauses](#json-where-clauses)
- [Get all the column names for a table](#get-all-the-column-names-for-a-table)
- [Compare the values of two columns](#compare-the-values-of-two-columns)
- [Accessor Caching](#accessor-caching)
- [New scalar() method](#new-scalar-method)
- [Select specific columns](#select-specific-columns)
- [Chain conditional clauses to the query without writing if-else statements](#chain-conditional-clauses-to-the-query-without-writing-if-else-statements)
- [Override Connection Attribute in Models](#override-connection-attribute-in-models)

### Reuse or clone query()

Usualmente necesitamos filtrar consultar muchas veces . Entonces muchas ocasiones usamos el método query.  En este ejemplo vamos a obtener los productos activos e inactivos creados hoy.

```php

$query = Product::query();


$today = request()->q_date ?? today();
if($today){
    $query->where('created_at', $today);
}

// obtenemos activos e inactivos productos.
$active_products = $query->where('status', 1)->get(); 
// esta linea modifica la variable donde contiene la consulta
$inactive_products = $query->where('status', 0)->get(); 
//aqui solo traeremos los productos inactivos
```

Sin embargo, después de obtener los `$active_products`, la variable `$query` será modificada. Por lo tanto, los `$inactive_products` no encontrarán ningún producto inactivo en `$query`, lo que resultará en una colección vacía cada vez. Esto se debe a que intentará buscar productos inactivos en `$active_products` (ya que `$query` solo contiene productos activos).

Para resolver este problema, podemos realizar consultas múltiples reutilizando el objeto `$query`. Por lo tanto, necesitamos clonar este objeto `$query` antes de realizar cualquier acción de modificación de la consulta (`$query`).
```php
$active_products = $query->clone()->where('status', 1)->get(); 
//No modificara la variable $query 
$inactive_products = $query->clone()->where('status', 0)->get(); 
// Entonces obtendremos los productos inactivos de la consulta.
```

### Remember to use bindings in your raw queries

Puedes pasar una matriz de variables a la mayoría de los métodos de consulta directa para evitar la inyección SQL.
```php
// Esto es vulnerable 
$fullname = request('full_name');
User::whereRaw("CONCAT(first_name, last_name) = $fullName")->get();

// Usando bindings
User::whereRaw("CONCAT(first_name, last_name) = ?", [request('full_name')])->get();
```

⭐ Aportación de [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1565806352219328513)

### Small cheat-sheet for using Full-Text Search with Laravel on MySQL

Migración
```php
Schema::create('comments', function (Blueprint $table) {
     $table->id();
     $table->string('title');
     $table->text('description');

     $table->fullText(['title', 'description']);
});
```

Lenguaje natural

Búsqueda por x cosa.

```php
Comment::whereFulltext(['title', 'description'], 'something')->get();
```

Lenguaje natural con la expansión de Query

Búsqueda por x cosa y usar el el resultado para realizar una consulta más larga.

```php
Comment::whereFulltext(['title', 'description'], 'something', ['expanded' => true])->get();
```

Modo booleano

El segundo argumento `'+something -else'` es la cadena de búsqueda que se utilizará. Utiliza el prefijo `+` para indicar que el término "something" debe estar presente y el prefijo `-` para indicar que el término "else" debe estar ausente. Esto se conoce como sintaxis de búsqueda booleana.

```php
Comment::whereFulltext(['title', 'description'], '+something -else', ['mode' => 'boolean'])->get();
```

⭐ Aportación de [@w3Nicolas](https://twitter.com/w3Nicolas/status/1566694849767772160/)

### Merging eloquent collections

El método de fusión (merge) de la colección Eloquent utiliza el ID para evitar modelos duplicados. 

Pero si estás fusionando colecciones de Modelos diferentes, puede provocar resultados inesperados.

En su lugar, utiliza el método de fusión (merge) de la colección base.

```php
$videos = Video::all();
$images = Image::all();
// Si hay videos con el mismo id como las imagenes serán remplazados
// Obtendrás los videos olvidados 
$allMedia = $videos->merge($images);


//Llama a `toBase()` en tu colección Eloquent para utilizar el método de fusión (merge) base en su lugar.
//
$allMedia = $videos->toBase()->merge($images);
```


⭐ Aportación de [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1568392184772296706)

### Perform operation without modifying updated_at field

Disponible desde Laravel 9.31.
Si deseas realizar operaciones en un modelo sin que se modifique su marca de tiempo `updated_at`, puedes operar en el modelo dentro de un cierre proporcionado al método `withoutTimestamps`.
```php
$user = User::first();

// `updated_at` is not changed...

User::withoutTimestamps(
     fn () => $user->update(['reserved_at' => now()])
);
```

⭐ Aportación de [@LaravelEloquent](https://twitter.com/LaravelEloquent/status/1573787406528126976)

### You can write transaction-aware code

Usando el método puedes escribir código que solo será ejecutado si la transacción es completada y si es descartada la transacción será deshecha.                                                                
```php
DB::transaction(function () {
     $user = User::create([...]);

     $user->teams()->create([...]);
});
```

```php
class User extends Model
{
     protected static function booted()
     {
          static::created(function ($user) {
               // Will send the email only if the
               // transaction is committed
               DB::afterCommit(function () use ($user) {
                    Mail::send(new WelcomeEmail($user));
               });
          });
     }
}
```


⭐ Aportación de [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1583960872602390528)

### Eloquent scopes inside of other relationships

¿Sabías  que puedes usar scopes de Eloquent dentro de relaciones?
**app/Models/Lesson.php**:
```php
public function scopePublished($query)
{
     return $query->where('is_published', true);
}
```

**app/Models/Course.php**:
```php
public function lessons(): HasMany
{
     return $this->hasMany(Lesson::class);
}

public function publishedLessons(): HasMany
{
     return $this->lessons()->published();
}
```

### New `rawValue()` method since Laravel 9.37


Laravel 9.37 tiene un nuevo método `rawValue()` para obtener un valor desde una expresión de SQL. Aquí hay algunos ejemplos del pull request:

```php
$first = TripModel::orderBy('date_at', 'ASC')
     ->rawValue('YEAR(`date_at`)');
$last = TripModel::orderBy('date_at', 'DESC')
     ->rawValue('YEAR(`date_at`)');

$fullname = UserModel::where('id', $id)
     ->rawValue('CONCAT(`first_name`, " ", `last_name`)');
```

⭐ Aportación de  [@LoydRG](https://twitter.com/LoydRG/status/1587689148768567298)

### Load data faster when the targeted value is an integer

En vez de  usar el método `whereIn()` para cargar grandes rangos de información cuando el valor objetivo es un número entero, usa el método  𝘄𝗵𝗲𝗿𝗲𝗜𝗻𝘁𝗲𝗴𝗲𝗿𝗜𝗻𝗥𝗮𝘄()  el cual es más rápido que 𝘄𝗵𝗲𝗿𝗲𝗜𝗻().

```php
// En vez de usar whereIN
Product::whereIn('id', range(1, 50))->get();

// usa WhereIntegerInRaw para más rapidez
Product::whereIntegerInRaw('id', range(1, 50))->get();
```

⭐ Aportación de   [@LaraShout](https://twitter.com/LaraShout)

### Load data completed between two timestamps

Usa el método 𝘄𝗵𝗲𝗿𝗲𝗕𝗲𝘁𝘄𝗲𝗲𝗻  para cargar registros entre dos datos timestamps. Además puedes pasar condiciones usando el operador **??**.
```php
// Cargar las tareas completadas de dos timestamps
Task::whereBetween('completed_at', [
    $request->from ?? '2023-01-01',
    $request->to ??  today()->toDateTimeString(),
]);
```

⭐ Aportación de [@LaraShout](https://twitter.com/LaraShout)

### Pass a raw query to order your results

Puedes pasar una query raw para ordenar resultados. Por ejemplo ordenar tareas por cuan largo ha sido la fecha de entrega que fue completada.

```php

// Ordenar las tareas por la tarea que fue completada antes de la fecha de entrega,
$tasks = Task::query()
    ->whereNotNull('completed_at')
    ->orderByRaw('due_at - completed_at DESC')
    ->get();
```

⭐ Aportación de  [@cosmeescobedo](https://twitter.com/cosmeescobedo)

### Eloquent where date methods

En Eloquent puedes hacer consultas con funciones como: `whereDay()`, `whereMonth()`, `whereYear()`, `whereDate()` y `whereTime()`.

```php
$products = Product::whereDate('created_at', '2018-01-31')->get();
$products = Product::whereMonth('created_at', '12')->get();
$products = Product::whereDay('created_at', '31')->get();
$products = Product::whereYear('created_at', date('Y'))->get();
$products = Product::whereTime('created_at', '=', '14:13:58')->get();
```

### Increments and decrements

SI quieres incrementar alguna columna sobre una tabla, solo necesitas usar la función  `increment()`. Ten en cuneta que no solo puedes incrementar 1 en 1 sino en el número que necesites como 50.

```php
Post::find($post_id)->increment('view_count');
User::find($user_id)->increment('points', 50);
```

### No timestamp columns

Si tu base de datos no contiene columnas timestamp(created_at, updated_at), puedes especificar que el modelo de Eloquent no los use con la propiedad `$timestamps = false` 

```php
class Company extends Model
{
    public $timestamps = false;
}
```

### Soft-deletes: multiple restore

Cuando usamos soft-deletes, puedes restaurar multiples registros en una sentencia.
```php
Post::onlyTrashed()->where('author_id', 1)->restore();
```

### Model all: columns

Cuando llamamos el método `all()` en cualquier Modelo podemos especificar las columnas.
```php
$users = User::all(['id', 'name', 'email']);
```

### To Fail or not to Fail

Además de el método  `findOrFail()`, en Eloquent está el método `firstOrFail()`  lo cual retorna la página 404 si no es encontrado el registro.

```php
$user = User::where('email', 'povilas@laraveldaily.com')->firstOrFail();
```

### Column name change

En Eloquent Query Builder puedes especificar dentro del método select la palabra "as" para retornar cualquier columna con un diferente nombre, como SQL de toda la vida.
```php
$users = DB::table('users')->select('name', 'email as user_email')->get();
```

### Map query results

Después de una consulta con Eloquent puedes modificar registros usando la función map sobre las colecciones.

```php
$users = User::where('role_id', 1)->get()->map(function (User $user) {
    $user->some_column = some_function($user);
    return $user;
});
```

### Change Default Timestamp Fields

¿Que si estas trabajando con una base de datos no nativa de Laravel y los campos timestamp se llaman diferente? Quizá puedes tener create_time y update_time. Con la Laravel puedes especificar el modelo también:
```php
class Role extends Model
{
    const CREATED_AT = 'create_time';
    const UPDATED_AT = 'update_time';
}
```

### Quick Order by created_at

En vez de esto: 

```php
User::orderBy('created_at', 'desc')->get();
```

Puedes usar esto:
```php
User::latest()->get();
```

Por defecto,   `latest()` ordenará por  `created_at`.
Hay un método al revés  `oldest()` el cual ordenará de forma ascendente`created_at`.
```php
User::oldest()->get();
```


Además puedes especificar otra columna para ordenarlo. Por ejemplo si quieres usar `updated_at` puedes hacer esto:
```php
$lastUpdatedUser = User::latest('updated_at')->first();
```

### Automatic Column Value When Creating Records

Si deseas generar un valor para una columna de la base de datos al crear un registro, puedes agregarlo al método `boot()` del modelo. Por ejemplo, si tienes un campo llamado "position" y quieres asignar la siguiente posición disponible al nuevo registro (como `Country::max('position') + 1`), puedes hacer lo siguiente:

```php
class Country extends Model {
    protected static function boot()
    {
        parent::boot();

        Country::creating(function($model) {
            $model->position = Country::max('position') + 1;
        });
    }
}
```

### DB Raw Query Calculations Run Faster


Usa SQL Raw queries como `whereRaw()` para hacer cálculos directamente en la consulta y no el Laravel así el resultado será más rápido. Por ejemplo si deseas obtener los usuarios que estuvieron activos después de su registro aquí está el código: 
```php
User::where('active', 1)
    ->whereRaw('TIMESTAMPDIFF(DAY, created_at, updated_at) > ?', 30)
    ->get();
```

### More than One Scope

Puedes combinar y unir Queries Scopes en Eloquent, usando más de un scope en la consulta.

Modelo:
```php
public function scopeActive($query) {
    return $query->where('active', 1);
}

public function scopeRegisteredWithinDays($query, $days) {
    return $query->where('created_at', '>=', now()->subDays($days));
}
```

Controlador: 

```php
$users = User::registeredWithinDays(30)->active()->get();
```

### No Need to Convert Carbon

Si estás realizando consultas con `whereDate()` y necesitas revisar los registros de hoy, puedes usar el helper de Carbon `now()`  y será automáticamente transformado a la fecha sin necesidad de usar `->toDateString()`.

```php
// En vez de 
$todayUsers = User::whereDate('created_at', now()->toDateString())->get();
// No se necesita  convertir.
$todayUsers = User::whereDate('created_at', now())->get();
```

### Grouping by First Letter

Puedes agrupar resultados Eloquent por cualquier condición personalizada, aquí está como puedes agrupar por la primera letra del nombre del usuario.
```php
$users = User::all()->groupBy(function($item) {
    return $item->name[0];
});
```

### Never Update the Column

Si tienes columnas y registros que quieres guardar y nunca más actualizar de nuevo, puedes colocar una restricción en el modelo Eloquent con un mutador:

- En versiones 9 and arriba:

```php
use Illuminate\Database\Eloquent\Casts\Attribute;

class User extends Model
{
    protected function email(): Attribute
    {
        return Attribute::make(
            set: fn ($value, $attributes) => $attributes['email'] ?? $value,
        );
    }
}
```

  En la versión 9 and anteriores:
```php
class User extends Model
{
    public function setEmailAttribute($value)
    {
        if (isset($this->attributes['email']) && ! is_null($this->attributes['email'])) {
            return;
        }
        $this->attributes['email'] = $value;
    }
}
```

### Find Many

El método `find()`  puede aceptar multiples parámetros y entonces retornar una colección de todos los registros encontrados no solo un modelo:
 
```php
// Retornará un modelo Eloquent
$user = User::find(1);
// Retornará una colleción
$users = User::find([1,2,3]);
```

```php
return Product::whereIn('id', $this->productIDs)->get();
// Puedes hacer esto
return Product::find($this->productIDs)
```

⭐ Aportación de [@tahiriqbalnajam](https://twitter.com/tahiriqbalnajam/status/1436120403655671817)

### Find Many and return specific columns

El método `find()`  puede aceptar multiples parámetros y entonces retornar una colección de todos los registros encontrados no solo un modelo con columnas especificas:
```php

// Retornará un Modelo ELoquent solo con dos columnas
$user = User::find(1, ['first_name', 'email']);

// Retornara una colección ELoquent solo con dos columnas
$users = User::find([1,2,3], ['first_name', 'email']);
```

⭐ Aportación de [@tahiriqbalnajam](https://github.com/tahiriqbalnajam)

### Find by Key

Puedes encontrar multiples registros con el método `whereKey()`  el cual se encarga de saber la llave primaria(id es la por defecto pero puedes sobrescribir la en el Modelo).
```php
$users = User::whereKey([1,2,3])->get();
```

### Use UUID instead of auto-increment

Si no necesitas usar el auto incrementing ID en tu modelo puedes usar un UUID.

Migration:

```php
Schema::create('users', function (Blueprint $table) {
    // $table->increments('id');
    $table->uuid('id')->unique();
});
```

#### Laravel 9 and above:

```php
use Illuminate\Database\Eloquent\Concerns\HasUuids;
use Illuminate\Database\Eloquent\Model;

class Article extends Model
{
    use HasUuids;

    // ...
}

$article = Article::create(['title' => 'Traveling to Europe']);

$article->id; // "8f8e8478-9035-4d23-b9a7-62f4d2612ce5"
```

#### Laravel 8 and below:

Model:

- En PHP 7.4.0 arriba:

```php
use Illuminate\Support\Str;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    public $incrementing = false;
    protected $keyType = 'string';

    protected static function boot()
    {
        parent::boot();

        self::creating(fn (User $model) => $model->attributes['id'] = Str::uuid());
        self::saving(fn (User $model) => $model->attributes['id'] = Str::uuid());
    }
}
```

- En PHP mas viejo  7.4.0:

```php
use Illuminate\Support\Str;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    public $incrementing = false;
    protected $keyType = 'string';

    protected static function boot()
    {
        parent::boot();

        self::creating(function ($model) {
             $model->attributes['id'] = Str::uuid();
        });
        self::saving(function ($model) {
             $model->attributes['id'] = Str::uuid();
        });
    }
}
```

### Sub-selects in Laravel Way

Desde Laravel 6 puedes usar la sentencia  `addSelect()` y realizar algún calculo a esa columna nueva.
```php
return Destination::addSelect(['last_flight' => Flight::select('name')
    ->whereColumn('destination_id', 'destinations.id')
    ->orderBy('arrived_at', 'desc')
    ->limit(1)
])->get();
```

### Hide Some Columns

Cuando consultamos con Eloquent query, podemos ocultar campos específicos de ser retornados, una de las maneras más rápidas es usar `->makeHidden()`.
```php
$users = User::all()->makeHidden(['email_verified_at', 'deleted_at']);
```

### Exact DB Error

Si desear encapsular error tipo Eloquent Query exceptions, usa el  `QueryException` en vez de usar Exception clase, con esto serás capaz de obtener el código SQL del error. 

```php
try {
    // Some Eloquent/SQL statement
} catch (\Illuminate\Database\QueryException $e) {
    if ($e->getCode() === '23000') { // integrity constraint violation
        return back()->withError('Invalid data');
    }
}
```

### Soft-Deletes with Query Builder


No olvides que los registros con soft-delete serán excluidos cuando usamos Eloquent, pero esto no aplicará cuando usemos  Query Builder. 

```php
// Excluirá registros con soft-deleted
$users = User::all();

// No excluirá registros con soft-deleted
$users = User::withTrashed()->get();

// No exluirá registros con soft-deleted
$users = DB::table('users')->get();
```

### Good Old SQL Query

Si deseas ejecutar una simple consulta SQL, sin obtener resultados, como cambiar o eliminar esquemas de base de datos, solamente necesitas usar `DB::statement()`.
```php
DB::statement('DROP TABLE users');
DB::statement('ALTER TABLE projects AUTO_INCREMENT=123');
```

### Use DB Transactions

Si tienes dos operaciones de base de datos realizadas y la segunda puede generar un error, ¿deberías deshacer la primera, verdad?
Para eso te sugiero usar DB Transactions, es bastante fácil en Laravel:

```php
DB::transaction(function () {
    DB::table('users')->update(['votes' => 1]);

    DB::table('posts')->delete();
});
```

### Update or Create

Si necesitas revisar si el registro existe y luego actualizar o crearlo puedes hacerlo en una sentencia `updateOrCreate()`:

```php
// En vez de esto
$flight = Flight::where('departure', 'Oakland')
    ->where('destination', 'San Diego')
    ->first();
if ($flight) {
    $flight->update(['price' => 99, 'discounted' => 1]);
} else {
    $flight = Flight::create([
        'departure' => 'Oakland',
        'destination' => 'San Diego',
        'price' => 99,
        'discounted' => 1
    ]);
}
// Haz esto en una sentencia
$flight = Flight::updateOrCreate(
    ['departure' => 'Oakland', 'destination' => 'San Diego'],
    ['price' => 99, 'discounted' => 1]
);
```


⭐ Aportación de [@pratiksh404](https://github.com/pratiksh404)

### Forget Cache on Save

Si tienes una clave de caché como `posts` que devuelve una colección, y deseas olvidar esa clave de caché en una nueva operación de almacenamiento o actualización, puedes llamar a la función estática `saved` en tu modelo.

```php
class Post extends Model
{
    public static function boot()
    {
        parent::boot();
        static::saved(function () {
           Cache::forget('posts');
        });
    }
}
```

⭐ Aportación de [@syofyanzuhad](https://github.com/syofyanzuhad)

### Change Format Of Created_at and Updated_at

Para cambiar el formato de fecha del campo `created_at`  puedes añadir el siguiente método en tu modelo así: 

Desde Laravel 9:
```php
protected function createdAtFormatted(): Attribute
{
    return Attribute::make(
        get: fn ($value, $attributes) => $attributes['created_at']->format('H:i d, M Y'),
    );
}
```

Laravel 8 para abajo:

```php
public function getCreatedAtFormattedAttribute()
{
   return $this->created_at->format('H:i d, M Y');
}
```


Entonces puedes usar  `$entry->created_at_formatted`  cuando sea necesario. Esto retornará el atributo `created_at`  así `04:19 23, Aug 2020`.

Y además para cambiar el formato del atributo `updated_at` puedes añadir este método:

Desde Laravel 9:
```php
protected function updatedAtFormatted(): Attribute
{
    return Attribute::make(
        get: fn ($value, $attributes) => $attributes['updated_at']->format('H:i d, M Y'),
    );
}
```

Laravel 8 para abajo:

```php
public function getUpdatedAtFormattedAttribute()
{
   return $this->updated_at->format('H:i d, M Y');
}
```

Entonces puedes usar  `$entry->updated_at_formatted`  cuando sea necesario. Esto retornará el atributo `created_at`  así `04:19 23, Aug 2020`.

⭐ Aportación de  [@pratiksh404](https://github.com/pratiksh404)

### Storing Array Type into JSON

Si tienes un input el cual toma un arreglo y tienes que guardarlo como JSON, puedes usar la propiedad  `$casts`  en tu modelo. Aqui las  `images`   es un atributo JSON.
```php
protected $casts = [
    'images' => 'array',
];
```

Así puedes guardarlo como JSON, pero cuando sea obtenido de la base de datos puede ser usado como un arreglo.

### Make a Copy of the Model

Si tienes dos modelos similares (dirección de envío y dirección de facturación) y necesitas hacer una copia de uno a otro, puedes usar el método `replicate()` y cambiar algunas propiedades después de eso.

Ejemplo de la [Documentación oficial](https://laravel.com/docs/8.x/eloquent#replicating-models):

```php
$shipping = Address::create([
    'type' => 'shipping',
    'line_1' => '123 Example Street',
    'city' => 'Victorville',
    'state' => 'CA',
    'postcode' => '90001',
]);

$billing = $shipping->replicate()->fill([
    'type' => 'billing'
]);

$billing->save();
```

### Reduce Memory

En ocasiones necesitamos cargar una larga cantidad de datos dentro de memoria. Por ejemplo:
```php
$orders = Order::all();
```

Pero eso puede hacer bastante lento si tenemos una muy grande cantidad de datos porque Laravel prepara los objetos de la clase Model. En estos casos, Laravel tiene una función `toBase()` muy práctica. 

```php
$orders = Order::toBase()->get();
//$orders contendrá la clase `Illuminate\Support\Collection` con los objetos  `StdClass`.
```


Llamando este método, obtendrá toda la información de la base de datos pero no serán clase Model. Ten en cuenta que es buena idea pasar un arreglo de campos al método get, para prevenir que todos los campos sean obtenidos desde la base de datos.

### Force query without $fillable/$guarded

Si creas una plantilla de Laravel como un "inicio" para otros desarrolladores y no tienes control sobre lo que ELLOS llenarían posteriormente en $fillable/$guarded del Modelo, puedes usar forceFill().

```php
$team->update(['name' => $request->name])
```


Si "name" no está en la propiedad `$fillable` del modelo Team o si no hay ninguna propiedad `$fillable/$guarded` en absoluto, entonces `forceFill()` no tendría ningún efecto.
```php
$team->forceFill(['name' => $request->name])
```


Esto ignorará el   `$fillable`  para esa única consulta y ejecutará no importa que. 

### 3-level structure of parent-children

Si tienes una estructura de 3 niveles de padre a hijos, como categorías en una e-shop y quieres mostrar el número de productos en el nivel, puedes usar  `with('yyy.yyy')` y después añadir `withCount()`   como condición.

```php
class HomeController extend Controller
{
    public function index()
    {
        $categories = Category::query()
            ->whereNull('category_id')
            ->with(['subcategories.subcategories' => function($query) {
                $query->withCount('products');
            }])->get();
    }
}
```

```php
class Category extends Model
{
    public function subcategories()
    {
        return $this->hasMany(Category::class);
    }

    public function products()
    {
        return $this->hasMany(Product::class);
    }
}
```

```php
<ul>
    @foreach($categories as $category)
        <li>
            {{ $category->name }}
            @if ($category->subcategories)
                <ul>
                @foreach($category->subcategories as $subcategory)
                    <li>
                        {{ $subcategory->name }}
                        @if ($subcategory->subcategories)
                            <ul>
                                @foreach ($subcategory->subcategories as $subcategory)
                                    <li>{{ $subcategory->name }} ({{ $subcategory->product_count }})</li>
                                @endforeach
                            </ul>
                        @endif
                    </li>
                @endforeach
                </ul>
            @endif
        </li>
    @endforeach
</ul>
```

### Perform any action on failure

Cuando buscamos un registro puede ser que queramos realizar algunas acciones si no es encontrado. Además de  `->firstOrFail()`  el cual retorna 404 puedes realizar cualquier acción cuando falla, solo utiliza `->firstOr(function() { ... })`
```php
$model = Flight::where('legs', '>', 3)->firstOr(function () {
    // ...
})
```

### Check if record exists or show 404

No uses find() para revisar si un registro existe. Usa findOrFail().

```php
$product = Product::find($id);
if (!$product) {
    abort(404);
}
$product->update($productDataArray);
```

Una manera más corta: 

```php
$product = Product::findOrFail($id); // shows 404 if not found
$product->update($productDataArray);
```

### Abort if condition failed

puede ser usado como una manera corta de verificar una condición y luego lanzar una página de excepción.

```php
$product = Product::findOrFail($id);
if($product->user_id != auth()->user()->id){
    abort(403);
}
```

Una manera más corta 

```php
/* abort_if(CONDITION, ERROR_CODE) */
$product = Product::findOrFail($id);
abort_if ($product->user_id != auth()->user()->id, 403)
```

### Fill a column automatically while you persist data to the database

Si deseas llenar una columna automáticamente mientras persistes datos en la base de datos (por ejemplo: slug), utiliza un observador de modelo en lugar de codificarlo manualmente cada vez.

```php
use Illuminate\Support\Str;

class Article extends Model
{
    ...
    protected static function boot()
    {
        parent:boot();

        static::saving(function ($model) {
            $model->slug = Str::slug($model->title);
        });
    }
}
```

⭐ Aportación de [@sky_0xs](https://twitter.com/sky_0xs/status/1432390722280427521)

### Extra information about the query

Puedes llamar el método  `explain()`  on consultas para saber información extra sobre la consulta realizada

```php
Book::where('name', 'Ruskin Bond')->explain()->dd();
```

```php
Illuminate\Support\Collection {#5344
    all: [
        {#15407
            +"id": 1,
            +"select_type": "SIMPLE",
            +"table": "books",
            +"partitions": null,
            +"type": "ALL",
            +"possible_keys": null,
            +"key": null,
            +"key_len": null,
            +"ref": null,
            +"rows": 9,
            +"filtered": 11.11111164093,
            +"Extra": "Using where",
        },
    ],
}
```

⭐ Aportación de  [@amit_merchant](https://twitter.com/amit_merchant/status/1432277631320223744)

### Using the doesntExist() method in Laravel

```php
// Esto funciona 
if ( 0 === $model->where('status', 'pending')->count() ) {
}

// No importa sobre el valor count(), solo necesito verificar si no hay algun registro
if ( ! $model->where('status', 'pending')->exists() ) {
}

// El simbolo de negación se ve muy feo entonces el méotodo doesntExist() hace este código más limpio.
if ( $model->where('status', 'pending')->doesntExist() ) {
}
```

⭐ Aportación de [@ShawnHooper](https://twitter.com/ShawnHooper/status/1435686220542234626)

### Trait that you want to add to a few Models to call their boot() method automatically

Si tu tienes un Trait que quieres añadir a algunos Modelos para llamar su método `boot()`  automáticamente, puedes añadir el método del Trait como boot[TraitName]
```php
class Transaction extends  Model
{
    use MultiTenantModelTrait;
}
```

```php
class Task extends  Model
{
    use MultiTenantModelTrait;
}
```

```php
trait MultiTenantModelTrait
{
    // EL nombre del método es boot[TraitName] y será auto llamado como  boot() en Transaction/Task 
    public static function bootMultiTenantModelTrait()
    {
        static::creating(function ($model) {
            if (!$isAdmin) {
                $isAdmin->created_by_id = auth()->id();
            }
        })
    }
}
```

### There are two common ways of determining if a table is empty in Laravel

Hay dos maneras de determinar si una tabla está vacía en Laravel. Llamando `exists()` o `count()`  directamente en el modelo.
Una retorna true/false y la otra retorna un entero el cual puedes usar como un condicional falso.


```php
public function index()
{
    if (\App\Models\User::exists()) {
        // retorna true o falso
    }

    if (\App\Models\User::count()) {
        // retorna el numero de registros en la tabla
    }
}
```

⭐ Aportación de  [@aschmelyun](https://twitter.com/aschmelyun/status/1440641525998764041)


### How to prevent “property of non-object” error

```php
// Digamos que tienes POst perteneciendo a AUthor y entonces el código es el siguiente:
$post->author->name;

// Claro, puedes prevenirlo así:
$post->author->name ?? ''
// o así
@$post->author->name


// Pero puedes hacerlo en el nivel de Eloquent relationship, esta relación retornara un modelo App/Author si nigun author es añadido a el post.

public function author() {
    return $this->belongsTo(Author::class)->withDefault();
}
// or
public function author() {
    return $this->belongsTo(Author::class)->withDefault([
        'name' => 'Guest Author'
    ]);
}
```

⭐ Aportación de [@aschmelyun](https://twitter.com/aschmelyun/status/1440641525998764041)

### Get original attributes after mutating an Eloquent record

Para obtener los atributos originales después de mutar el registro Eloquent solo necesitas llamar 
getOriginal('atributo').

```php
$user = App\User::first();
$user->name; // John
$user->name = "Peter"; // Peter
$user->getOriginal('name'); // John
$user->getOriginal(); // Original $user record
```

⭐ Aportación de [@devThaer](https://twitter.com/devThaer/status/1442133797223403521)

### A simple way to seed a database

Una manera simple de llenar una base de datos en Laravel con un archivo .sql:

```php
DB::unprepared(
    file_get_contents(__DIR__ . './dump.sql')
);
```

⭐ Aportación de  [@w3Nicolas](https://twitter.com/w3Nicolas/status/1447902369388249091)

### The crossJoinSub method of the query constructor

CROSS JOIN es útil cuando se necesita combinar todos los registros de una tabla con los resultados de otra consulta.

```php
use Illuminate\Support\Facades\DB;

$totalQuery = DB::table('orders')->selectRaw('SUM(price) as total');

DB::table('orders')
    ->select('*')
    ->crossJoinSub($totalQuery, 'overall')
    ->selectRaw('(price / overall.total) * 100 AS percent_of_total')
    ->get();
```

⭐ Aportación de [@PascalBaljet](https://twitter.com/pascalbaljet)

### Belongs to Many Pivot table naming

Para determinar el nombre de la tabla des relaciones intermedias, Eloquent unirá los dos nombres de los modelos relacionados en orden alfábetico.

```php
class Post extends Model
{
    public $table = 'posts';

    public function tags()
    {
        return $this->belongsToMany(Tag::class);
    }
}
```


Sin embargo, eres libre de sobrescribir esta opción solo necesitarías especificar la tabla de union en el segundo argumento.

```php
class Post extends Model
{
    public $table = 'posts';

    public function tags()
    {
        return $this->belongsToMany(Tag::class, 'posts_tags');
    }
}
```


SI deseas ser explicito sobre las llaves primarias puedes también suplirlos como tercer y cuarto argumento.
```php
class Post extends Model
{
    public $table = 'posts';

    public function tags()
    {
        return $this->belongsToMany(Tag::class, 'post_tag', 'post_id', 'tag_id');
    }
}
```

⭐ Aportación de [@iammikek](https://twitter.com/iammikek)

### Order by Pivot Fields

`BelongsToMany::orderByPivot()`  permite directamente ordenar los resultados de la consulta a la relación BelongsToMany.

```php
class Tag extends Model
{
    public $table = 'tags';
}

class Post extends Model
{
    public $table = 'posts';

    public function tags()
    {
        return $this->belongsToMany(Tag::class, 'post_tag', 'post_id', 'tag_id')
            ->using(PostTagPivot::class)
            ->withTimestamps()
            ->withPivot('flag');
    }
}

class PostTagPivot extends Pivot
{
    protected $table = 'post_tag';
}

// Somewhere in the Controller
public function getPostTags($id)
{
    return Post::findOrFail($id)->tags()->orderByPivot('flag', 'desc')->get();
}
```

⭐ Aportación de [@PascalBaljet](https://twitter.com/pascalbaljet)

### Find a single record from a database

El método retornara solo un registro que cumple el criterio. Si ningún registro es encontrado entonces será lanzada una excepción tipo  `NoRecordsFoundException` . Si multiples registros son encontrados, entonces otra excepción será lanzada `MultipleRecordsFoundException` .

```php
DB::table('products')->where('ref', '#123')->sole();
```

⭐ Aportación de  [@PascalBaljet](https://twitter.com/pascalbaljet)

### Automatic records chunking

Similar a el método pero más fácil de usar. Automáticamente separa los resultados en partes (chunks).

```php
return User::orderBy('name')->chunkMap(fn ($user) => [
    'id' => $user->id,
    'name' => $user->name,
]), 25);
```

⭐ Aportación de  [@PascalBaljet](https://twitter.com/pascalbaljet)

### Updating the model without dispatching events

Ocasionalmente necesitas actualizar un modelo sin enviar eventos.  Podemos hacer esto con el método  `updateQuietly()` , el cual por debajo usa el método `saveQuietly()` 

```php
$flight->updateQuietly(['departed' => false]);
```

⭐ Aportación de  [@PascalBaljet](https://twitter.com/pascalbaljet)

### Periodic cleaning of models from obsolete records

Para limpiar periódicamente limpiar registros de modelos obsoletos con este Trait se puede realizar automáticamente, solo necesitas ajustar la frecuencia del `model:prune` comando en la clase Kernel.
```php
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Prunable;
class Flight extends Model
{
    use Prunable;
    /**
     * Get the prunable model query.
     *
     * @return \Illuminate\Database\Eloquent\Builder
     */
    public function prunable()
    {
        return static::where('created_at', '<=', now()->subMonth());
    }
}
```

También, en el método pruning puedes colocar acciones que deben ser realizadas antes de borrar el modelo:
```php
protected function pruning()
{
    // Remover adicionales recursos

    Storage::disk('s3')->delete($this->filename);
}
```

⭐ Aportación de  [@PascalBaljet](https://twitter.com/pascalbaljet)

### Immutable dates and casting to them

Laravel 8.53 introduce los casteos `immutable_date` e `immutable_datetime` que convierten las fechas en `Immutable`.

Convertir a Carbon Immutable en lugar de una instancia regular de Carbon.

```php
class User extends Model
{
    public $casts = [
        'date_field'     => 'immutable_date',
        'datetime_field' => 'immutable_datetime',
    ];
}
```

⭐ Aportación de  [@PascalBaljet](https://twitter.com/pascalbaljet)

### The findOrFail method also accepts a list of ids

El método findOrFail acepta una lista de ids. SI alguno de esos ids no son encontrados entonces falla.
Interesante si necesitas obtener un set de específicos modelos y no quieres revisar si el número de id enviados son los mismos que recibes.

```php
User::create(['id' => 1]);
User::create(['id' => 2]);
User::create(['id' => 3]);

// Retorna el usuario
$user = User::findOrFail(1);

// Lanza una excepción
User::findOrFail(99);

// Retorna los 3
$users = User::findOrFail([1, 2, 3]);

// Lanza una excepción porque es incapaz de encontrar todos los usuarios.
User::findOrFail([1, 2, 3, 99]);
```

⭐ Aportación de [@timacdonald87](https://twitter.com/timacdonald87/status/1457499557684604930)

### Prunable trait to automatically remove models from your database

Novedad en Laravel 8.50: puedes usar el rasgo Prunable para eliminar automáticamente modelos de tu base de datos. Por ejemplo, puedes eliminar permanentemente modelos eliminados suavemente después de unos días.
```php
class File extends Model
{
    use SoftDeletes;

    use Prunable;

    public function prunable()
    {
        return static::query()->where('deleted_at', '<=', now()->subDays(14));
    }

    protected function pruning()
    {
        //Elimina el archivo de s3 antes de elimnar el modelo
        Storage::disk('s3')->delete($this->filename);
    }
}

// Añade el comando a tu schedule
$schedule->command(PruneCommand::class)->daily();
```

⭐ Aportación de [@Philo01](https://twitter.com/Philo01/status/1457626443782008834)

### withAggregate method

Bajo el capó, los métodos withAvg/withCount/withSum y otros en Eloquent utilizan el método 'withAggregate'. Puedes utilizar este método para agregar una subconsulta basada en una relación.
```php
// Eloquent Model
class Post extends Model
{
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}

$posts = Post::with('user')->get();

$posts = Post::withAggregate('user', 'name')->get();

$posts->first()->user_name;
```

⭐ Aportación de [@pascalbaljet](https://twitter.com/pascalbaljet/status/1457702666352594947)

### Date convention

Utilizar la convención `something_at` en lugar de solo un booleano en los modelos de Laravel te brinda visibilidad sobre cuándo se modificó una bandera, como cuando un producto es activo.

```php
// Migration
Schema::table('products', function (Blueprint $table) {
    $table->datetime('live_at')->nullable();
});

public function live()
{
    return !is_null($this->live_at);
}

protected $dates = [
    'live_at'
];
```

⭐ Aportación de [@alexjgarrett](https://twitter.com/alexjgarrett/status/1459174062132019212)

### Eloquent multiple upserts

El método upsert() insertará o actualizará múltiples registros.

- Primer arreglo: los valores a insertar o actualizar
- Segundo: columnas identificadoras únicas utilizadas en la instrucción select
- Tercero: columnas que deseas actualizar si el registro existe

```php
Flight::upsert([
    ['departure' => 'Oakland', 'destination' => 'San Diego', 'price' => 99],
    ['departure' => 'Chicago', 'destination' => 'New York', 'price' => 150],
], ['departure', 'destination'], ['price']);
```

⭐ Aportación de [@mmartin_joo](https://twitter.com/mmartin_joo/status/1461591319516647426)

### Retrieve the Query Builder after filtering the results

Para recuperar el generador de consultas (Query Builder) después de filtrar los resultados, puedes usar `->toQuery()`.

El método internamente utiliza el primer modelo de la colección y una comparación `whereKey` en los modelos de la colección.
```php
// Recuperar todos los usuarios con sesión iniciada
$loggedInUsers = User::where('logged_in', true)->get();

// Filtrarlos usando un método de colección o filtrado en PHP
$nthUsers = $loggedInUsers->nth(3);

// No puedes hacer esto en la colección
$nthUsers->update(/* ... */);

// Pero puedes recuperar el generador de consultas (Builder) usando ->toQuery()
if ($nthUsers->isNotEmpty()) {
    $nthUsers->toQuery()->update(/* ... */);
}
```

⭐ Aportación de [@RBilloir](https://twitter.com/RBilloir/status/1462529494917566465)

### Custom casts

Puedes crear casteos personalizados para que Laravel formatee automáticamente los datos de tu modelo Eloquent. Aquí tienes un ejemplo que capitaliza el nombre de un usuario cuando se recupera o se cambia.

```php
class CapitalizeWordsCast implements CastsAttributes
{
    public function get($model, string $key, $value, array $attributes)
    {
        return ucwords($value);
    }

    public function set($model, string $key, $value, array $attributes)
    {
        return ucwords($value);
    }
}

class User extends Model
{
    protected $casts = [
        'name'  => CapitalizeWordsCast::class,
        'email' => 'string',
    ];
}
```

⭐ Aportación de [@mattkingshott](https://twitter.com/mattkingshott/status/1462828232206659586)

### Order based on a related model's average or count
  
¿Alguna vez has necesitado ordenar según el promedio o la cantidad de un modelo relacionado?

¡Es fácil con Eloquent!

```php
public function bestBooks()
{
    Book::query()
        ->withAvg('ratings as average_rating', 'rating')
        ->orderByDesc('average_rating');
}
```

⭐ Aportación de [@mmartin_joo](https://twitter.com/mmartin_joo/status/1466769691385335815)

### Return transactions result


Si tienes una transacción de base de datos y deseas devolver su resultado, hay al menos dos formas. Observa el ejemplo:
```php
// 1.- puedes pasar el parametro por referencia
$invoice = NULL;
DB::transaction(function () use (&$invoice) {
    $invoice = Invoice::create(...);
    $invoice->items()->attach(...);
})

// 2.- solo retornar el resultado:
$invoice = DB::transaction(function () {
    $invoice = Invoice::create(...);
    $invoice->items()->attach(...);

    return $invoice;
});
```

### Remove several global scopes from query

Cuando utilizas los Alcances Globales (Global Scopes) de Eloquent, no solo puedes usar MÚLTIPLES alcances, sino que también puedes eliminar ciertos alcances cuando no los necesitas, proporcionando un arreglo a `withoutGlobalScopes()`.

[Link to docs](https://laravel.com/docs/8.x/eloquent#removing-global-scopes)

```php
// Remover todos los scopes globales
User::withoutGlobalScopes()->get();

// Remover algunos de los scopes globales
User::withoutGlobalScopes([
    FirstScope::class, SecondScope::class
])->get();
```

### Order JSON column attribute

With Eloquent you can order results by a JSON column attribute

Con Eloquent puedes ordenar resultados por el atributo de una columna tipo JSON
```php
// JSON column example:
// bikes.settings = {"is_retired": false}
$bikes = Bike::where('athlete_id', $this->athleteId)
        ->orderBy('name')
        ->orderByDesc('settings->is_retired')
        ->get();
```

Tip given by [@brbcoding](https://twitter.com/brbcoding/status/1473353537983856643)

⭐ Aportación de [@brbcoding](https://twitter.com/brbcoding/status/1473353537983856643)
### Get single column's value from the first result

Puedes usar el método para obtener solo el valor de una columna del primer resultado de una consulta.

```php
// En vez de esto
Integration::where('name', 'foo')->first()->active;

// Usa esto
Integration::where('name', 'foo')->value('active');

// o esto para lanzar una excepción si no se encuentra
Integration::where('name', 'foo')->valueOrFail('active')';
```

⭐ Aportación de  [@justsanjit](https://twitter.com/justsanjit/status/1475572530215796744)

### Check if altered value changed key

¿Alguna vez has querido saber si los cambios que has realizado en un modelo han alterado el valor de una clave? No hay problema, simplemente usa `originalIsEquivalent`.

```php
$user = User::first(); // ['name' => "John']

$user->name = 'John';

$user->originalIsEquivalent('name'); // true

$user->name = 'David'; // Set directly
$user->fill(['name' => 'David']); // Or set via fill

$user->originalIsEquivalent('name'); // false
```

⭐ Aportación de [@mattkingshott](https://twitter.com/mattkingshott/status/1475843987181379599)

### New way to define accessor and mutator

Nueva forma de definir accesores y mutadores de atributos en Laravel 8.77:

```php
// Antes se tenía que usar dos métodos
public function setTitleAttribute($value)
{
    $this->attributes['title'] = strtolower($value);
}
public function getTitleAttribute($value)
{
    return strtoupper($value);
}

// Nuevo
protected function title(): Attribute
{
    return new Attribute(
        get: fn ($value) => strtoupper($value),
        set: fn ($value) => strtolower($value),
    );
}
```

⭐ Aportación de [@Teacoders](https://twitter.com/Teacoders/status/1473697808456851466)

### Another way to do accessors and mutators

En caso de que vayas a utilizar los mismos accesores y mutadores en muchos modelos, puedes usar casteos personalizados en su lugar.

Simplemente crea una `clase` que implemente la interfaz `CastsAttributes`. La clase debe tener dos métodos: el primero es `get` para especificar cómo se deben recuperar los modelos de la base de datos y el segundo es `set` para especificar cómo se almacenará el valor en la base de datos.

```php
<?php

namespace App\Casts;

use Carbon\Carbon;
use Illuminate\Contracts\Database\Eloquent\CastsAttributes;

class TimestampsCast implements CastsAttributes
{
    public function get($model, string $key, $value, array $attributes)
    {
        return Carbon::parse($value)->diffForHumans();
    }

    public function set($model, string $key, $value, array $attributes)
    {
        return Carbon::parse($value)->format('Y-m-d h:i:s');
    }
}

```

Entonces puedes implementar el cast en la clase model.

```php
<?php

namespace App\Models;

use Illuminate\Foundation\Auth\User as Authenticatable;
use App\Casts\TimestampsCast;
use Carbon\Carbon;


class User extends Authenticatable
{

    /**
     * The attributes that should be cast.
     *
     * @var array
     */
    protected $casts = [
        'updated_at' => TimestampsCast::class,
        'created_at' => TimestampsCast::class,
    ];
}

```

⭐ Aportación de [@AhmedRezk](https://github.com/AhmedRezk59)

### When searching for the first record, you can perform some actions

Cuando buscas el primer registro y deseas realizar algunas acciones cuando no lo encuentras, `firstOrFail()` lanza una excepción 404.

En su lugar, puedes usar `firstOr(function() {})`. Laravel tiene todo cubierto.

```php
$book = Book::whereCount('authors')
            ->orderBy('authors_count', 'DESC')
            ->having('modules_count', '>', 10)
            ->firstOr(function() {

            });
```

⭐ Aportación de [@bhaidar](https://twitter.com/bhaidar/status/1487757487566639113/)

¿Sabías que puedes convertir directamente la fecha de `created_at` a un formato legible para humanos como "hace 1 minuto" o "hace 1 mes" utilizando la función `diffForHumans()`? Laravel Eloquent, de forma predeterminada, habilita la instancia de Carbon en el campo `created_at`.

```php
$post = Post::whereId($id)->first();
$result = $post->created_at->diffForHumans();

/* OUTPUT */
// 1 Minutes ago, 2 Week ago etc..as per created time
```

⭐ Aportación de [@vishal\_\_2931](https://twitter.com/vishal__2931/status/1488369014980038662)

### Ordering by an Eloquent Accessor

¡Ordenar por un Accesor de Eloquent! Sí, es posible. En lugar de ordenar por el accesor a nivel de base de datos, ordenamos por el accesor en la colección devuelta.

```php
class User extends Model
{
    // ...
    protected $appends = ['full_name'];

    // Since Laravel 9
    protected function full_name(): Attribute
    {
        return Attribute::make(
            get: fn ($value, $attributes) => $attributes['first_name'] . ' ' . $attributes['last_name'];),
        );
    }

    // Laravel 8 and lower
    public function getFullNameAttribute()
    {
        return $this->attribute['first_name'] . ' ' . $this->attributes['last_name'];
    }
    // ..
}
```

```php
class UserController extends Controller
{
    // ..
    public function index()
    {
        $users = User::all();

        $users->sortByDesc('full_name');


        $users->sortBy('full_name');

        // ..
    }
    // ..
}
```

`sortByDesc` y `sortBy`  son métodos de Collection

⭐ Aportación de [@bhaidar](https://twitter.com/bhaidar/status/1490671693618053123)

### Check for specific model was created or found

Si deseas verificar si un modelo específico fue creado o encontrado, utiliza el atributo de modelo `wasRecentlyCreated`.
```php
$user = User::create([
    'name' => 'Oussama',
]);

// return boolean
return $user->wasRecentlyCreated;

// true for recently created
// false for found (already on you db)
```

⭐ Aportación de [@sky_0xs](https://twitter.com/sky_0xs/status/1491141790015320064)

### Laravel Scout with database driver

Con Laravel v9, puedes utilizar Laravel Scout (Búsqueda) con el controlador de base de datos. ¡Ya no más "where likes"!

```php
$companies = Company::search(request()->get('search'))->paginate(15);
```

⭐ Aportación de [@magarrent](https://twitter.com/magarrent/status/1493221422675767302)

### Make use of the value method on the query builder


Haz uso del método `value` en el generador de consultas (query builder) para ejecutar una consulta más eficiente cuando solo necesitas recuperar una única columna.
```php
// Antes (obtiene todas las columnas)
Statistic::where('user_id', 4)->first()->post_count;

// Después (obtiene solo `post_count`)
Statistic::where('user_id', 4)->value('post_count');
```

⭐ Aportación de [@mattkingshott](https://twitter.com/mattkingshott/status/1493583444244410375)

### Pass array to where method

En Laravel puedes pasar un arreglo al método where

```php
// En vez de esto
JobPost::where('company', 'laravel')
        ->where('job_type', 'full time')
        ->get();

// Puedes psara un arreglo
JobPost::where(['company' => 'laravel',
                'job_type' => 'full time'])
        ->get();
```

⭐ Aportación de [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1495626752282234881)

### Return the primary keys from models collection

¿Sabías que existe el método `modelsKeys()` en la colección de Eloquent? Devuelve las claves primarias de la colección de modelos.

```php
$users = User::active()->limit(3)->get();

$users->modelsKeys(); // [1, 2, 3]
```


⭐ Aportación de [@iamharis010](https://twitter.com/iamharis010/status/1495816807910891520)

### Force Laravel to use eager loading

Si deseas evitar la carga diferida (lazy loading) en tu aplicación, solo necesitas agregar la siguiente línea al método `boot()` en tu `AppServiceProvider`.

```php
Model::preventLazyLoading();
```

Pero, si deseas habilitar esta función solo en tu entorno de desarrollo local, puedes cambiar el código anterior de la siguiente manera:

```php
Model::preventLazyLoading(!app()->isProduction());
```

⭐ Aportación de  [@CatS0up](https://github.com/CatS0up)

### Make all your models mass assignable

No es un enfoque recomendado por razones de seguridad, pero es posible.

Cuando deseas hacer esto, no necesitas establecer un arreglo `$guarded` vacío para cada modelo, como este:

```php
protected $guarded = [];
```

Puedes hacerlo desde un único lugar, simplemente agrega la siguiente línea al método `boot()` en tu `AppServiceProvider`:

```php
Model::unguard();
```

⭐ Aportación de  [@CatS0up](https://github.com/CatS0up)

### Hiding columns in select all statements

Si utilizas Laravel v8.78 y MySQL 8.0.23 en adelante, puedes definir columnas seleccionadas como "invisibles". Las columnas definidas como `invisible` serán ocultadas de las declaraciones `select *`.

Sin embargo, para lograr esto, debemos utilizar el método `invisible()` en la migración, algo así:

```php
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

Schema::table('table', function (Blueprint $table) {
    $table->string('secret')->nullable()->invisible();
});
```

⭐ Aportación de [@CatS0up](https://github.com/CatS0up)
### JSON Where Clauses

Laravel ofrece helpers para consultar columnas tipo JSON a bases de datos lo soportan. Actualmente  MySQL 5.7+, PostgreSQL, SQL Server 2016, and SQLite 3.9.0 (usan JSON1 extension)
```php
// Para consultar una columna json puedes usar el operador ->
$users = User::query()
            ->where('preferences->dining->meal', 'salad')
            ->get();
// Puedes revisar si un arreglo en JSON contiene ciertos valores
$users = User::query()
            ->whereJsonContains('options->languages', [
                'en', 'de'
               ])
            ->get();
// Puedes también consultar por el ancho del arreglo
$users = User::query()
            ->whereJsonLength('options->languages', '>', 1)
            ->get();
```

⭐ Aportación de [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1509663119311663124)

### Get all the column names for a table

Obtén todos los nombres de las columnas de una tabla.

```php
DB::getSchemaBuilder()->getColumnListing('users');
/*
returns [
    'id',
    'name',
    'email',
    'email_verified_at',
    'password',
    'remember_token',
    'created_at',
    'updated_at',
];
*/
```

⭐ Aportación de [@aaronlumsden](https://twitter.com/aaronlumsden/status/1511014229737881605)

### Compare the values of two columns

Puedes usar el método `whereColumn`  para comparar los valores de dos columnas.
```php
return Task::whereColumn('created_at', 'updated_at')->get();
// pass a comparison operator
return Task::whereColumn('created_at', '>', 'updated_at')->get();
```

⭐ Aportación de [@iamgurmandeep](https://twitter.com/iamgurmandeep/status/1511673260353548294)

### Accessor Caching

En Laravel 9.6, si tienes  un accessor intensivo en cómputo, puedes usar el método shouldCache.

```php
public function hash(): Attribute
{
    return Attribute::make(
        get: fn($value) => bcrypt(gzuncompress($value)),
    )->shouldCache();
}
```

⭐ Aportación de  [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1514304409563402244)

### New scalar() method

El Laravel 9.8.0 el método `scalar()` permite que obtengas la primera columna de la primera fila de tu consulta.
```php
// Before
DB::selectOne("SELECT COUNT(CASE WHEN food = 'burger' THEN 1 END) AS burgers FROM menu_items;")->burgers
// Now
DB::scalar("SELECT COUNT(CASE WHEN food = 'burger' THEN 1 END) FROM menu_items;")
```

⭐ Aportación de  [@justsanjit](https://twitter.com/justsanjit/status/1514550185837408265)

### Select specific columns

Para seleccionar columnas especificas en un modelo puedes usar el método `select` o puedes colocar un arreglo directamente al método `get()`.

```php
$employees = Employee::select(['name', 'title', 'email'])->get();
$employees = Employee::get(['name', 'title', 'email']);
```

⭐ Aportación de [@ecrmnn](https://twitter.com/ecrmnn/status/1516087672351203332)

### Chain conditional clauses to the query without writing if-else statements

El helper "when" in the query builder resulta de lo mejor. Puedes encadenar clausulas condicionales al query sin usar if-else. Esto hace bastante limpio el código.
```php
class RatingSorter extends Sorter
{
    function execute(Builder $query)
    {
        $query
            ->selectRaw('AVG(product_ratings.rating) AS avg_rating')
            ->join('product_ratings', 'products.id', '=', 'product_ratings.product_id')
            ->groupBy('products.id')
            ->when(
                $this->direction === SortDirections::Desc,
                fn () => $query->orderByDesc('avg_rating')
                fn () => $query->orderBy('avg_rating'),
            );

        return $query;
    }
}
```

⭐ Aportación de [@mmartin_joo](https://twitter.com/mmartin_joo/status/1521461317940350976)

### Override Connection Attribute in Models

Sobrescribir el atributo de conexión de base de datos para modelos individuales en Laravel resulta una técnica poderosa. Aquí hay algunos casos de uso donde puedes descubrir lo práctico que resulta: 

#### 1. Multiple Database Connections

Si tu aplicación usa multiples conexiones de bases de datos (Mysql, PostgresSql o diferentes instancias de la misma base de datos) puedes especificar qué conexión debería ser usada para un modelo en particular. Sobrescribiendo la propiedad `$connection` puedes fácilmente administrar esas conexiones y asegurarte que tus modelos están interactuando con la apropiada base de datos.

#### 2. Data Sharding

En casos donde usas fragmentación de datos para distribuir datos a través de multiples bases de datos, quizá tengas diferentes modelos que apunten a diferentes conexiones. 

#### 3. Third-Party Integration

Cuando integramos servicios de terceros que proveen su propia base de datos, quizá necesitas usar una conexión específica para el modelo que representa esa información desde ese servicio.

#### 4. Multi-Tenancy Applications

En una aplicación multi-tenant , es posible tener bases de datos separadas para cada inquilino(tenant). Al sobrescribir dinámicamente la propiedad `$connection` en tus modelos, puedes cambiar fácilmente entre las bases de datos de los inquilinos según el usuario actual, lo que garantiza el aislamiento de datos y una correcta gestión de recursos.
```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class CustomModel extends Model
{
    protected $connection = 'your_custom_connection';
}
```