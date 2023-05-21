## Models Relations

⬆️ [Menú principal](README.md#laravel-tips) ⬅️ [Anterior (DB Models and Eloquent)](db-models-and-eloquent.md) ➡️ [SIguiente (Migrations)](migrations.md)

- [OrderBy on Eloquent relationships](#orderby-on-eloquent-relationships)
- [Add where statement to Many-to-Many relation](#add-where-statement-to-many-to-many-relation)
- [Get the newest (or oldest) item of another relation](#get-the-newest-or-oldest-item-of-another-relation)
- [Conditional relationships](#conditional-relationships)
- [Raw DB Queries: havingRaw()](#raw-db-queries-havingraw)
- [Eloquent has() deeper](#eloquent-has-deeper)
- [Has Many. How many exactly?](#has-many-how-many-exactly)
- [Default model](#default-model)
- [Use hasMany to create Many](#use-hasmany-to-create-many)
- [Multi level Eager Loading](#multi-level-eager-loading)
- [Eager Loading with Exact Columns](#eager-loading-with-exact-columns)
- [Touch parent updated_at easily](#touch-parent-updated_at-easily)
- [Always Check if Relationship Exists](#always-check-if-relationship-exists)
- [Use withCount() to Calculate Child Relationships Records](#use-withcount-to-calculate-child-relationships-records)
- [Extra Filter Query on Relationships](#extra-filter-query-on-relationships)
- [Load Relationships Always, but Dynamically](#load-relationships-always-but-dynamically)
- [Instead of belongsTo, use hasMany](#instead-of-belongsto-use-hasmany)
- [Rename Pivot Table](#rename-pivot-table)
- [Update Parent in One Line](#update-parent-in-one-line)
- [Laravel 7+ Foreign Keys](#laravel-7-foreign-keys)
- [Combine Two "whereHas"](#combine-two-wherehas)
- [Check if Relationship Method Exists](#check-if-relationship-method-exists)
- [Pivot Table with Extra Relations](#pivot-table-with-extra-relations)
- [Load Count on-the-fly](#load-count-on-the-fly)
- [Randomize Relationship Order](#randomize-relationship-order)
- [Filter hasMany relationships](#filter-hasmany-relationships)
- [Filter by many-to-many relationship pivot column](#filter-by-many-to-many-relationship-pivot-column)
- [A shorter way to write whereHas](#a-shorter-way-to-write-wherehas)
- [You can add conditions to your relationships](#you-can-add-conditions-to-your-relationships)
- [New `whereBelongsTo()` Eloquent query builder method](#new-wherebelongsto-eloquent-query-builder-method)
- [The `is()` method of one-to-one relationships for comparing models](#the-is-method-of-one-to-one-relationships-for-comparing-models)
- [`whereHas()` multiple connections](#wherehas-multiple-connections)
- [Update an existing pivot record](#update-an-existing-pivot-record)
- [Relation that will get the newest (or oldest) item](#relation-that-will-get-the-newest-or-oldest-item)
- [Replace your custom queries with ofMany](#replace-your-custom-queries-with-ofmany)
- [Avoid data leakage when using orWhere on a relationship](#avoid-data-leakage-when-using-orwhere-on-a-relationship)

### OrderBy on Eloquent relationships

Pudes espeicificar  `orderBy()`  para order dada la columna, directamente en las relaciones Eloquent.

```php
public function products()
{
    return $this->hasMany(Product::class);
}

public function productsByName()
{
    return $this->hasMany(Product::class)->orderBy('name');
}
```

### Add where statement to Many-to-Many relation

En las relaciones muchos a muchos, puedes añadir clausulas `where` a las tablas intermedias (pivot) usando el método  `wherePivot`.
```php
class Developer extends Model
{
     // Get all clients related to this developer
     public function clients()
     {
          return $this->belongsToMany(Clients::class);
     }

     // Get only local clients
     public function localClients()
     {
          return $this->belongsToMany(Clients::class)
               ->wherePivot('is_local', true);
     }
}
```

⭐ Aportación de  [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1582904416457269248)

### Get the newest (or oldest) item of another relation


Desde Laravel 8.42, en los modelos Eloquent puedes definir una relación para traer los más nuevos o los más viejos.

```php
/**
 * Los usuarios más nuevos
 */
public function latestOrder()
{
    return $this->hasOne(Order::class)->latestOfMany();
}

/**
 * Los uusarios mas antiguo
 */
public function oldestOrder()
{
    return $this->hasOne(Order::class)->oldestOfMany();
}
```

### Conditional relationships

Si te das cuenta que usas las mismas relaciones con propiedades similares como "where" puedes  crear una nueva relación con ellas mismas. 

Modelo:
```php
public function comments()
{
    return $this->hasMany(Comment::class);
}

public function approved_comments()
{
    return $this->hasMany(Comment::class)->where('approved', 1);
}
```

### Raw DB Queries: havingRaw()

Puedes usar consultas `Raw DB` en varios lugares, funciones incluidas como`havingRaw()` después `groupBy()`.

```php
Product::groupBy('category_id')->havingRaw('COUNT(*) > 1')->get();
```

### Eloquent has() deeper

Puedes usar la función Eloquent `has()` para hacer queries en las relaciones en más de dos niveles de profundidad.

```php
// Author -> hasMany(Book::class);
// Book -> hasMany(Rating::class);
$authors = Author::has('books.ratings')->get();
```

### Has Many. How many exactly?

En las relaciones Eloquent `hasMany()`  puedes filtrar los registros que tienen X cantidades de relaciones.
```php
// Author -> hasMany(Book::class)
$authors = Author::has('books', '>', 5)->get();
```

### Default model


Puedes asignar un modelo por defecto en las relaciones `belongsTo`, para evitar errores fatales cuando llamamos las relaciones como:  `{{ $post->user->name }}`, si el $post->user no existe.

```php
public function user()
{
    return $this->belongsTo(User::class)->withDefault();
}
```

### Use hasMany to create Many


Si tienes realciones `hasMany()` , puedes usar el método `saveMany()` para guardar multiples entradas desde el modelo padre al modelo hijo con una sola   linea.
```php
$post = Post::find(1);
$post->comments()->saveMany([
    new Comment(['message' => 'First comment']),
    new Comment(['message' => 'Second comment']),
]);
```

### Multi level Eager Loading


En Laravel puedes realizar Eager Loading en multiples niveles de relaciones, por ejemplo: cargamos a el modelo Book su author y dentro del author su relación país:
```php
$users = Book::with('author.country')->get();
```

### Eager Loading with Exact Columns

Cuando realizamos Eager Loading podemos especificar qué columnas debemos traer nada más.
```php
$users = Book::with('author:id,name')->get();
```

Incluso podemos hacerlolo  en un segundo nivel:

```php
$users = Book::with('author.country:id,name')->get();
```

### Touch parent updated_at easily


Si quieres actuaizar un registro y asu vez actualizar la columna `updated_at` de la relación padre, solo usa la propiedad `$touches = ['post'];` en el modelo hijo .
```php
class Comment extends Model
{
    protected $touches = ['post'];
}
```

### Always Check if Relationship Exists

Nunca accedas a un campo de la relación hijo ( `$model->relationship->field` ) sin antes revisar si la relación existe.

Podrían haberlo borrado por la razón que sea.

Podrías utilizar una condicional como:  `{{ $model->relationship->field ?? '' }}` en Blade o `{{ optional($model->relationship)->field }}`. Con la llegada de PHP 8.0 existe nullsafe operator que funciona así: `{{ $model->relationship?->field) }}`

### Use withCount() to Calculate Child Relationships Records


Si tienes relaciones `hasMany()` y quieres calcular cuantas relaciones hijas contiene, no necesitas escribir consultas especiales. Por ejemplo si tienes  posts y comentarios en el modelo Usuario, escribes  `withCount()`:

```php
public function index()
{
    $users = User::withCount(['posts', 'comments'])->get();
    return view('users', compact('users'));
}
```


Para usar los resultados de la consulta, en las vistas accedemos a la variable:
`{relationship}_count` :

```blade
@foreach ($users as $user)
<tr>
    <td>{{ $user->name }}</td>
    <td class="text-center">{{ $user->posts_count }}</td>
    <td class="text-center">{{ $user->comments_count }}</td>
</tr>
@endforeach
```

También puedes ordernar el campo así:
```php
User::withCount('comments')->orderBy('comments_count', 'desc')->get();
```

### Extra Filter Query on Relationships

Cuando traes relaciones a los modelos, puedes especificar algunos limitadores u ordenarlos en una función de cierre. Por ejemplo: si quieres obtener todos los Paises con los mayores habitantes aquí está el código:
```php
$countries = Country::with(['cities' => function($query) {
    $query->orderBy('population', 'desc');
}])->get();
```

### Load Relationships Always, but Dynamically

Tu solo no puedes especificar qué relaciones **SIEMPRE** traer en tus modelos, sino también hacerlo dinamicamente en el método constructor

```php
class ProductTag extends Model
{
    protected $with = ['product'];

    public function __construct() {
        parent::__construct();
        $this->with = ['product'];

        if (auth()->check()) {
            $this->with[] = 'user';
        }
    }
}
```

### Instead of belongsTo, use hasMany


Cuando usamos la relación `belongsTo` , en vez de pasar el id padre cuando creamos registros hijos podemos usar  la relación `hasMany` para hacer más cortas las lineas de código. 

```php
// if Post -> belongsTo(User), and User -> hasMany(Post)...
// Then instead of passing user_id...
Post::create([
    'user_id' => auth()->id(),
    'title' => request()->input('title'),
    'post_text' => request()->input('post_text'),
]);

// Do this
auth()->user()->posts()->create([
    'title' => request()->input('title'),
    'post_text' => request()->input('post_text'),
]);
```

### Rename Pivot Table

Si necesitas renombrar la plabra "pivot" de las relaciones intermedias, especifica en la relación `->as('name')`.

Modelo:
```php
public function podcasts() {
    return $this->belongsToMany(Podcast::class)
        ->as('subscription')
        ->withTimestamps();
}
```

Controlador:
```php
$podcasts = $user->podcasts();
foreach ($podcasts as $podcast) {
    // instead of $podcast->pivot->created_at ...
    echo $podcast->subscription->created_at;
}
```

### Update Parent in One Line

Si tienes una relacion  `belongsTo()` , puedes actualizar el modelo  en una sola sentencia.

```php
// if Project -> belongsTo(User::class)
$project->user->update(['email' => 'some@gmail.com']);
```

### Laravel 7+ Foreign Keys

Desde Laravel 7 en las migraciones no necesitas escribir dos lineas donde creas la columna y después asignas la tabla foranea, en vez utiliza el método `foreignId()`. ``

```php
//Antes de Laravel 7
Schema::table('posts', function (Blueprint $table)) {
    $table->unsignedBigInteger('user_id');
    $table->foreign('user_id')->references('id')->on('users');
}

// Desde Laravel 7
Schema::table('posts', function (Blueprint $table)) {
    $table->foreignId('user_id')->constrained();
}

// o si tu campo es diferente de la tabla de referencia
Schema::table('posts', function (Blueprint $table)) {
    $table->foreignId('created_by_id')->constrained('users', 'column');
}
```

### Combine Two "whereHas"

En Eloquent puedes combinar el método  `whereHas()` y  `orDoesntHave()` en una consulta.
```php
User::whereHas('roles', function($query) {
    $query->where('id', 1);
})
->orDoesntHave('roles')
->get();
```

### Check if Relationship Method Exists

Si en tus relaciones de Eloquent los nombres son dinámicos y necesitas revisar si la relación existe en el objeto, usa la función `method_exists($object, $methodName)` de PHP.
```php
$user = User::first();
if (method_exists($user, 'roles')) {
    // Do something with $user->roles()->...
}
```

### Pivot Table with Extra Relations

In las relaciones muchos a muchos, la tabla pivot puede contener campos extras e incluso extra relaciones de otros modelos.

Entonces necesitamos generar por seperado un Modelo que apunte comot abla Pivot. 

```bash
php artisan make:model RoleUser --pivot
```

Después, especificamos en la relación `belongsToMany()` con el método `->using()`. Entonces así podemos hacer la mágia, como en este ejemplo:  

```php
// in app/Models/User.php
public function roles()
{
    return $this->belongsToMany(Role::class)
        ->using(RoleUser::class)
        ->withPivot(['team_id']);
}

// app/Models/RoleUser.php: notice extends Pivot, not Model
use Illuminate\Database\Eloquent\Relations\Pivot;

class RoleUser extends Pivot
{
    public function team()
    {
        return $this->belongsTo(Team::class);
    }
}

// Then, in Controller, you can do:
$firstTeam = auth()->user()->roles()->first()->pivot->team->name;
```

### Load Count on-the-fly

Además en el método `withCount()`  Eloquent que sirve para contar los registros de la relación, pero tambíen puedes cargarlo hasta que lo necesites con  `loadCount()`:
```php
//si tu modelo Book tiene muchos Reviews
$book = Book::first();

$book->loadCount('reviews');
// Entonces obtienes acceso a $book->reviews_count;
// O incluso con una condición extra
$book->loadCount(['reviews' => function ($query) {
    $query->where('rating', 5);
}]);
```

### Randomize Relationship Order

Puedes usar  `inRandomOrder()` para ordenar de forma aleatoria los resultados, pero también Laravel es capaz de ordenar de forma aleatoria  las relaciones.

```php
// SI tienes un examen y quieres hacer aleatoria las preguntas

// 1. Si quieres obtener las preguntas en order aleatorio:
$questions = Question::inRandomOrder()->get();

// 2. Si quieres además obtener las preguntas en orden aleatorio y también sus opciones:
$questions = Question::with(['answers' => function($q) {
    $q->inRandomOrder();
}])->inRandomOrder()->get();
```

### Filter hasMany relationships

Aquí te presento un código de mi proyecto como ejemplo, mostrando la posibilidad de filtrar las relaciones hasMany. 

TagTypes -> hasMany Tags -> hasMany Examples

Y si quieres consultar todos los tipos que tengan sus Tags, pero solo esos que tengan Examples, odenandolo por los que contienen más Examples.
```php
$tag_types = TagType::with(['tags' => function ($query) {
    $query->has('examples')
        ->withCount('examples')
        ->orderBy('examples_count', 'desc');
    }])->get();
```

### Filter by many-to-many relationship pivot column

Si tienes  tienes relaciones muchas a muchas, en tabla pivot solo muestra las llaves foraneas ysi necesitas añadir una columna  existente solo añde `withPivot`.

```php
class Tournament extends Model
{
    public function countries()
    {
        return $this->belongsToMany(Country::class)->withPivot(['position']);
    }
}
```

```php
class TournamentsController extends Controller
{
    public function whatever_method() {
        $tournaments = Tournament::with(['countries' => function($query) {
            $query->orderBy('position');
        }])->latest()->get();
    }
}
```

### A shorter way to write whereHas

Una manera de escribir whereHas() más corto y con una simple condición apartir de Laravel 8.57.

```php
// Before
User::whereHas('posts', function ($query) {
    $query->where('published_at', '>', now());
})->get();

// After
User::whereRelation('posts', 'published_at', '>', now())->get();
```

### You can add conditions to your relationships

```php
class User
{
    public function posts()
    {
        return $this->hasMany(Post::class);
    }

    // with a getter
    public function getPublishedPostsAttribute()
    {
        return $this->posts->filter(fn ($post) => $post->published);
    }

    // with a relationship
    public function publishedPosts()
    {
        return $this->hasMany(Post::class)->where('published', true);
    }
}
```

⭐ Aportación de [@anwar_nairi](https://twitter.com/anwar_nairi/status/1441718371335114756)

### New `whereBelongsTo()` Eloquent query builder method

Con la llegada de Laravel 8.63.0 nos trae un  método `whereBelongsTo()` Eloquent query builder . Esto nos permite eliminar los nombres de las llaves foraneas en nuestras consultas y usar los métodos inertes de las relaciones más limpio.

```php
// From:
$query->where('author_id', $author->id)

// To:
$query->whereBelongsTo($author)

// Easily add more advanced filtering:
Post::query()
    ->whereBelongsTo($author)
    ->whereBelongsTo($cateogry)
    ->whereBelongsTo($section)
    ->get();

// Specify a custom relationship:
$query->whereBelongsTo($author, 'author')
```

⭐ Aportación de  [@danjharrin](https://twitter.com/danjharrin/status/1445406334405459974)

### The `is()` method of one-to-one relationships for comparing models

Ahora podemos hacer comparaciones entre modelos sin hacer más consultas a base de datos.

```php
// Antes: la llave foranea es tomada del modelo Post
$post->author_id === $user->id;

// Antes: una petición adicional es hecha para obtener el modelo User desde la relación Autor
$post->author->is($user);

// DESPUES
$post->author()->is($user);
```

⭐ Aportación de @PascalBaljet](https://twitter.com/pascalbaljet)

### `whereHas()` multiple connections

Con el método whereHas() además de poder realizar  consultas más especificas, también podemos especificar qué conexiones.

```php
// User Model
class User extends Model
{
    protected $connection = 'conn_1';

    public function posts()
    {
        return $this->hasMany(Post::class);
    }
}

// Post Model
class Post extends Model
{
    protected $connection = 'conn_2';

    public function user()
    {
        return $this->belongsTo(User::class, 'user_id');
    }
}

// wherehas()
$posts = Post::whereHas('user', function ($query) use ($request) {
      $query->from('db_name_conn_1.users')->where(...);
  })->get();
```

⭐ Aportación de [@PascalBaljet](https://twitter.com/pascalbaljet)

### Update an existing pivot record

Si necesitas actualizar un registro intermedio en la tabla pivot, usa el método  `updateExistingPivot` en vez de `syncWithPivotValues`.

```php
// Migraciones
Schema::create('role_user', function ($table) {
    $table->unsignedId('user_id');
    $table->unsignedId('role_id');
    $table->timestamp('assigned_at');
})

// Primer parametro para el id del registro y el segundo para actualizar las columnas
$user->roles()->updateExistingPivot(
    $id, ['assigned_at' => now()],
);
```

⭐ Aportación de [@sky_0xs](https://twitter.com/sky_0xs/status/1461414850341621760)

### Relation that will get the newest (or oldest) item

Ahora en Laravel 8.32 en los modelos Eloquent podemos definir una relación que obtenga los ulitmos(los mas viejos) registros.

```php
public function historyItems(): HasMany
{
    return $this
        ->hasMany(ApplicationHealthCheckHistoryItem::class)
        ->orderByDesc('created_at');
}

public function latestHistoryItem(): HasOne
{
    return $this
        ->hasOne(ApplicationHealthCheckHistoryItem::class)
        ->latestOfMany();
}
```

### Replace your custom queries with ofMany

```php
class User extends Authenticable {
    // Get most popular post of user
	//Obtiene los post con más likes    
    public function mostPopularPost() {
        return $this->hasOne(Post::class)->ofMany('like_count', 'max');
    }
}
```

⭐ Aportación de[@LaravelEloquent](https://twitter.com/LaravelEloquent/status/1493324310328578054)

### Avoid data leakage when using orWhere on a relationship

```php
$user->posts()
    ->where('active', 1)
    ->orWhere('votes', '>=', 100)
    ->get();
```

Retorna: todos los post donde los votos son mayores o iguales a 100.

```sql
select * from posts where user_id = ? and active = 1 or votes >= 100
```

```php
use Illuminate\Database\Eloquent\Builder;

$users->posts()
    ->where(function (Builder $query) {
        return $query->where('active', 1)
                    ->orWhere('votes', '>=', 100);
    })
    ->get();
```

Retorna: todos los post donde los votos son mayores o iguales a 100.


```sql
select * from posts where user_id = ? and (active = 1 or votes >= 100)
```

⭐ Aportación de  [@BonnickJosh](https://twitter.com/BonnickJosh/status/1494779780562096139)
