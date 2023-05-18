## Migrations

⬆️ [Menú principal](README.md#laravel-tips) ⬅️ [Anterior (Models Relations)](models-relations.md) ➡️ [Siguiente (Views)](views.md)

- [Order of Migrations](#order-of-migrations)
- [Migration fields with timezones](#migration-fields-with-timezones)
- [Database migrations column types](#database-migrations-column-types)
- [Default Timestamp](#default-timestamp)
- [Migration Status](#migration-status)
- [Create Migration with Spaces](#create-migration-with-spaces)
- [Create Column after Another Column](#create-column-after-another-column)
- [Make migration for existing table](#make-migration-for-existing-table)
- [Output SQL before running migrations](#output-sql-before-running-migrations)
- [Anonymous Migrations](#anonymous-migrations)
- [You can add "comment" about a column inside your migrations](#you-can-add-comment-about-a-column-inside-your-migrations)
- [Checking For Table / Column Existence](#checking-for-table--column-existence)
- [Group Columns within an After Method](#group-columns-within-an-after-method)
- [Add the column in the database table only if it's not present & can drop it if, its present](#add-the-column-in-the-database-table-only-if-its-not-present--can-drop-it-if-its-present)
- [Method to set the default value for current timestamp](#method-to-set-the-default-value-for-current-timestamp)

### Order of Migrations

If you want to change the order of DB migrations, just rename the file's timestamp, like from `2018_08_04_070443_create_posts_table.php` to`2018_07_04_070443_create_posts_table.php` (changed from `2018_08_04` to `2018_07_04`).

They run in alphabetical order.

Si quieres cambiar el order de las migraciones, solo renombra el timestamp del archivo, por ejemplo: de  `2018_08_04_070443_create_posts_table.php` a `2018_07_04_070443_create_posts_table.php` (cambio desde  `2018_08_04` a `2018_07_04`).

Las migraciones son leídas orden alfabetico.

### Migration fields with timezones

Did you know that in migrations there's not only `timestamps()` but also `timestampsTz()`, for the timezone?

¿Sabías que en las migraciones no solo éxiste el atributo `timestamps()`sino también  `timestampsTz()`, para el timezone (añade el timezone en la columna)

```php
Schema::create('employees', function (Blueprint $table) {
    $table->increments('id');
    $table->string('name');
    $table->string('email');
    $table->timestampsTz();
});
```

Also, there are columns `dateTimeTz()`, `timeTz()`, `timestampTz()`, `softDeletesTz()`.

También hay  columnas como: ` dateTimeTz()`,` timeTz()`,` timestampTz()`,` softDeletesTz()`.

### Database migrations column types

There are interesting column types for migrations, here are a few examples.

Hay interesantes tipos de columnas para las migraciones, aquí hay algunos ejemplos:

```php
$table->geometry('positions');
$table->ipAddress('visitor');
$table->macAddress('device');
$table->point('position');
$table->uuid('id');
```

See all column types on the [official documentation](https://laravel.com/docs/master/migrations#creating-columns).

MIra todos los tipos en la [official documentation]([Database: Migrations - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/master/migrations#creating-columns)).

### Default Timestamp

While creating migrations, you can use `timestamp()` column type with option
`useCurrent()` and `useCurrentOnUpdate()`, it will set `CURRENT_TIMESTAMP` as default value.

Mientras creamos las migraciones, puedes usar la columna`timestamp()` con la opción `useCurrent()` y `useCurrentOnUpdate()`, esto colocará el  `CURRENT_TIMESTAMP` como valor por defecto

```php
$table->timestamp('created_at')->useCurrent();
$table->timestamp('updated_at')->useCurrentOnUpdate();
```

### Migration Status

If you want to check what migrations are executed or not yet, no need to look at the database "migrations" table, you can launch `php artisan migrate:status` command.

Si necesitas checar qué migraciones ya han sido ejecutadas o no, no hay necesidad de mirar en la base de datos la tabla de migraciones,  solo necesitas el comando `php artisan migrate:status`

Example result:

Ejemplo:

```
Migration name .......................................................................... Batch / Status  
2014_10_12_000000_create_users_table ........................................................... [1] Ran  
2014_10_12_100000_create_password_resets_table ................................................. [1] Ran  
2019_08_19_000000_create_failed_jobs_table ..................................................... [1] Ran    
```

### Create Migration with Spaces

When typing `make:migration` command, you don't necessarily have to use underscore `_` symbol between parts, like `create_transactions_table`. You can put the name into quotes and then use spaces instead of underscores.

Cuando ejectuamos el comando `make:migration`no necesariamente tienes que usar el guión bajo `_`  para dividirlas palabras. En vez de eso simplemente coloca el nombre entre doble commilas.

```php
// This works
// Esto funciona
php artisan make:migration create_transactions_table
// También funciona
// But this works too
php artisan make:migration "create transactions table"
```

⭐Aportación de: [Steve O on Twitter](https://twitter.com/stephenoldham/status/1353647972991578120)

### Create Column after Another Column

_Notice: Only MySQL

_Aviso: Solo en MySQL

If you're adding a new column to the existing table, it doesn't necessarily have to become the last in the list. You can specify after which column it should be created:

Si estas añadiendo una nueva columa a una tabla ya existente, no necesariamente tiene que convertirse en la ultima en la lista. Puedes especificar despues de qué columa deberia ser creada

```php
Schema::table('users', function (Blueprint $table) {
    $table->string('phone')->after('email');
});
```

If you want your column to be the first in your table , then use the first method.

Si solo quieres que tu columna sea la prima en tu tabla, entonces usa el método `first()`

```php
Schema::table('users', function (Blueprint $table) {
    $table->string('uuid')->first();
});
```

Also the `after()` method can now be used to add multiple fields.

Además el método `after()` ahora puede ser usado para añadir multiples campos.

```php
Schema::table('users', function (Blueprint $table) {
    $table->after('remember_token', function ($table){
        $table->string('card_brand')->nullable();
        $table->string('card_last_four', 4)->nullable();
    });
});
```

### Make migration for existing table

If you make a migration for existing table, and you want Laravel to generate the Schema::table() for you, then add "\_in_xxxxx_table" or "\_to_xxxxx_table" at the end, or specify "--table" parameter.
`php artisan change_fields_products_table` generates empty class

Si quieres hacer una migracion para una tabla existente y quieres que laravel genere el Sachema::table() automaticamente, entonces añade "__in_xxx_table" o "_to_xxxx_table" y al final añade el parametro "--table".

```php
class ChangeFieldsProductsTable extends Migration
{
    public function up()
    {
        //
    }
}
```

But add `in_xxxxx_table` `php artisan make:migration change_fields_in_products_table` and it generates class with `Schema::table()` pre-filled

Al añadir `in_xxxxx_table` `php artisan make:migration change_fields_in_products_table` genera la clase con `Schema::table()`pre llenada.

```php
class ChangeFieldsProductsTable extends Migration
{
    public function up()
    {
        Schema::table('products', function (Blueprint $table) {
            //
        })
    };
}
```

Also you can specify `--table` parameter `php artisan make:migration whatever_you_want --table=products`

También puedes especificar el parametro`--table` en  `php artisan make:migration lo_que_quieras --table=products`

```php
class WhateverYouWant extends Migration
{
    public function up()
    {
        Schema::table('products', function (Blueprint $table) {
            //
        })
    };
}
```

### Output SQL before running migrations

When typing `migrate --pretend` command, you get the SQL query that will be executed in the terminal. It's an interesting way to debug SQL if necessary.

Cuando colocamos el comando `php artisan migrate` en conjunto con el parametro `--pretend` nos retorna la query que será ejecutada en la termina. Esto es bastante interesante cuando se trata de debugear a nivel de base de datos.

```php
// Artisan command
php artisan migrate --pretend
```

Tip given by [@zarpelon](https://github.com/zarpelon)

⭐Aportación de [@zarpelon](https://github.com/zarpelon)

### Anonymous Migrations

The Laravel team released Laravel 8.37 with anonymous migration support, which solves a GitHub issue with migration class name collisions. The core of the problem is that if multiple migrations have the same class name, it'll cause issues when trying to recreate the database from scratch.
Here's an example from the [pull request](https://github.com/laravel/framework/pull/36906) tests:

EL equipo de laravel ha lanzado soporte a las migraciones anonimas para Laravel 8.37, lo cual resulve un Github Issue acerca del choque producido por tener el mismo nombre en las clases. EL origen de este problema esque si multiples migraciones tienen el mismo nombre en la clase, esto causa problemas cuando tratamos de correr migraciones desde 0.

Mas sobre el [tema.](https://github.com/laravel/framework/pull/36906)

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration {
    public function up(
    {
        Schema::table('people', function (Blueprint $table) {
            $table->string('first_name')->nullable();
        });
    }

    public function down()
    {
        Schema::table('people', function (Blueprint $table) {
            $table->dropColumn('first_name');
        });
    }
};
```

Tip given by [@nicksdot](https://twitter.com/nicksdot/status/1432340806275198978)

⭐Aportación de [@nicksdot](https://twitter.com/nicksdot/status/1432340806275198978)

### ###You can add "comment" about a column inside your migrations

You can add "comment" about a column inside your migrations and provide useful information.

Puedes añadir comentarios sobre una columna dentro de tus migraciones para guardar información sobre esa columna.

If database is managed by someone other than developers, they can look at comments in Table structure before performing any operations.

Si la base de datos es administrada por alguien mas que no es desarrollador, puede darle un vistazo a la estructura antes de realizar cualquier accion.

```php
$table->unsignedInteger('interval')
    ->index()
    ->comment('This column is used for indexing.')
```

Tip given by [@nicksdot](https://twitter.com/nicksdot/status/1432340806275198978)

⭐Aportación de [@nicksdot](https://twitter.com/nicksdot/status/1432340806275198978)

### Checking For Table / Column Existence

You may check for the existence of a table or column using the hasTable and hasColumn methods:

Puedes revisar la existencia de una tabla o columna usando el método `hasTable` y `hasColumn`

```php
if (Schema::hasTable('users')) {
    // The "users" table exists...
}

if (Schema::hasColumn('users', 'email')) {
    // The "users" table exists and has an "email" column...
}
```

Tip given by [@dipeshsukhia](https://github.com/dipeshsukhia)

⭐ Aportación de [@dipeshsukhia](https://github.com/dipeshsukhia)

### Group Columns within an After Method

In your migrations, you can add multiple columns after another column using the after method:

En tus migraciones puedes añadir multples columnas después de otras columnas usnado el método after:

```php
Schema::table('users', function (Blueprint $table) {
    $table->after('password', function ($table) {
        $table->string('address_line1');
        $table->string('address_line2');
        $table->string('city');
    });
});
```

Tip given by [@ncosmeescobedo](https://twitter.com/cosmeescobedo/status/1512233993176973314)

⭐ Aportación de [@ncosmeescobedo](https://twitter.com/cosmeescobedo/status/1512233993176973314)

### Add the column in the database table only if it's not present & can drop it if, its present

Now you can add the column in the database table only if its not present & can drop it if, its present. For that following methods are introduced:

👉 whenTableDoesntHaveColumn

👉 whenTableHasColumn

Available from Laravel 9.6.0

Ahora puedes añadir la columna en base de datos **SI NO ESTA PRESENTE** y  eliminarla **SI EXISTE**. Los siguientes metodos están disponibles para dicha acción:

👉 whenTableDoesntHaveColumn

👉 whenTableHasColumn

Disponbile apartir de Laravel 9.6.0

```php
return new class extends Migration {
    public function up()
    {
        Schema::whenTableDoesntHaveColumn('users', 'name', function (Blueprint $table) {
            $table->string('name', 30);
        });
    }

    public function down()
    {
        Schema::whenTableHasColumn('users', 'name', function (Blueprint $table) {
            $table->dropColumn('name');
        });
    }
}
```

Tip given by [@iamharis010](https://twitter.com/iamharis010/status/1510579415163432961)

⭐ Aportación de [@iamharis010](https://twitter.com/iamharis010/status/1510579415163432961)

### Method to set the default value for current timestamp

You can use `useCurrent()` method for your custom timestamp column to store the current timestamp as a default value.

Puedes usar  el método `useCUrrent` para tus columnas timestamp personalizados  para guardar el actual timestamp por defecto

```php
Schema::create('posts', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->timestamp('added_at')->useCurrent();
    $table->timestamps();
});
```

Tip given by [@iamgurmandeep](https://twitter.com/iamgurmandeep/status/1517152425748148225)

⭐ Aportación de [@iamgurmandeep](https://twitter.com/iamgurmandeep/status/1517152425748148225)
