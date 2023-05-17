## Collections

⬆️ [Menu principal](README.md#laravel-tips) ⬅️ [Anterior (Validations)](validation.md) ➡️ [SIguiente (Auth)](auth.md)

- [Use groupBy on Collections with Custom Callback Function](#use-groupby-on-collections-with-custom-callback-function)
- [Laravel Scopes can be combined using "Higher Order" orWhere Method](#laravel-scopes-can-be-combined-using-higher-order-orwhere-method)
- [Multiple Collection Methods in a Row](#multiple-collection-methods-in-a-row)
- [Calculate Sum with Pagination](#calculate-sum-with-pagination)
- [Serial no in foreach loop with pagination](#serial-no-in-foreach-loop-with-pagination)
- [Higher order collection message](#higher-order-collection-message)
- [Get an existing key or insert a value if it doesn't exist and return the value](#get-an-existing-key-or-insert-a-value-if-it-doesnt-exist-and-return-the-value)
- [Static times method](#static-times-method)

### Use groupBy on Collections with Custom Callback Function

Si quieres agrupar resultados por alguna condición que no sea por una columna  directa en la base de datos, puedes conseguirlo con una function de clausura/cierre.

Por ejemplo si necesitas agrupar a todos los usuarios por el dia en que se registraron en la aplicacion el código sería así:

```php
$users = User::all()->groupBy(function($item) {
    return $item->created_at->format('Y-m-d');
});
```

⚠️ Aviso: En el ejemplo anterior se usa la clase `Collection` , la agrupación de datos por la condción se realiza **DESPUES** de que los resultados son obetenios de la base de datos.

### Laravel Scopes can be combined using "Higher Order" orWhere Method

El siguiente ejemplo pertenece a la documentación.

Antes:

```php
User::popular()->orWhere(function (Builder $query) {
     $query->active();
})->get()
```

Después:

```php
User::popular()->orWhere->active()->get();
```

⭐ Aportación de [@TheLaravelDev](https://twitter.com/TheLaravelDev/status/1564608208102199298/) 

### Multiple Collection Methods in a Row

Si realizas un consulta para obtener todos los resultados con `->all()` o `->get()`, después puedes realizar varias operaciones de agregación con el mismo resultado, no  se tiene que realizar varias peticiones a la base de datos cada vez que se use una  funcion de agregación.

```php
$users = User::all();
echo 'Max ID: ' . $users->max('id');
echo 'Average age: ' . $users->avg('age');
echo 'Total budget: ' . $users->sum('budget');
```

✅Algunas [funciones de agregación]([Database: Query Builder - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/10.x/queries#aggregates)) son: `count`, `max`, `min`, `avg` y `sum`. 

### Calculate Sum with Pagination

Como realizar la suma de todos los registros cuando tiene paginados los resultados. Para esto debes hacer la agregación ANTES de la paginación pero en la misma consulta.

```php
// Como conseguir la suma de los Post con paginación?
$posts = Post::paginate(10);
// La suma solo será de la pagina 1, no de todos los Post
$sum = $posts->sum('post_views');

// Usa Query builder
$query = Post::query();
// Calculamos la suma
$sum = $query->sum('post_views');
// Después hacemos la paginación en la misma consulta 
$posts = $query->paginate(10);
```

### Serial no in foreach loop with pagination

Es posible utilizar el índice de los elementos de una colección en un bucle "foreach" como número de serie (SL) en la paginación

```php
   ...
   <th>Serial</th>
    ...
    @foreach ($products as $product)
    <tr>
        <td>{{ $loop->index + $product->firstItem() }}</td>
        ...
    @endforeach
```

Esto resolverá el problema de que en las páginas siguientes (?page=2&...), el índice comience desde el punto en el que se detuvo.

### Higher order collection message

Las colecciones también brindan soporte para "mensajes de orden superior", que son atajos para realizar acciones comunes en las colecciones.

En este ejemplo se caclula el precio por grupo de productos en la variable offer

```php
$offer = [
        'name'  => 'offer1',
        'lines' => [
            ['group' => 1, 'price' => 10],
            ['group' => 1, 'price' => 20],
            ['group' => 2, 'price' => 30],
            ['group' => 2, 'price' => 40],
            ['group' => 3, 'price' => 50],
            ['group' => 3, 'price' => 60]
        ]
];

$totalPerGroup = collect($offer['lines'])->groupBy->group->map->sum('price');
```

### Get an existing key or insert a value if it doesn't exist and return the value

En Laravel 8.81 las colleciones aceptan el metodo `getOrPut` que simplifica el caso de uso donde ya sea que obtengas una llave existente o quieras insertar un valor en caso de que no exista y retornar lo al mismo tiempo. 

```php
$key = 'name';
// Buena opcion pero se puede mejorar...
if ($this->collection->has($key) === false) {
    $this->collection->put($key, ...);
}

return $this->collection->get($key);

// Usando el metodo 'getOrPut()' con una función
return $this->collection->getOrPut($key, fn() => ...);
// O pasar un valor por defecto
return $this->collection->getOrPut($key, $value='teacoders');
```

⭐Aportación de  [@Teacoders](https://twitter.com/Teacoders/status/1488338815592718336)  

### Static times method

El método estático `times` crea una nueva collecion al invokar la clausura proporcionada un número específico de veces.

```php
Collection::times(7, function ($number) {
    return now()->addDays($number)->format('d-m-Y');
});
// Resultado: [01-04-2022, 02-04-2022, ..., 07-04-2022]
```

⭐Aportación de  [@Teacoders ](https://twitter.com/Teacoders/status/1509447909602906116) 
