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

Si quieres cambiar el order de las migraciones, solo renombra el timestamp del archivo, por ejemplo: de  `2018_08_04_070443_create_posts_table.php` a `2018_07_04_070443_create_posts_table.php` (cambio desde  `2018_08_04` a `2018_07_04`).

Las migraciones son leídas orden alfabetico.

### Migration fields with timezones

¿Sabías que en las migraciones no solo éxiste el atributo `timestamps()`sino también  `timestampsTz()`, para el timezone (añade el timezone en conjunto con la fecha )?

```php
Schema::create('employees', function (Blueprint $table) {
    $table->increments('id');
    $table->string('name');
    $table->string('email');
    $table->timestampsTz();
});
```



También hay  columnas como: ` dateTimeTz()`,` timeTz()`,` timestampTz()`,` softDeletesTz()`.

### Database migrations column types

Hay interesantes tipos de columnas para las migraciones, aquí hay algunos ejemplos:

```php
$table->geometry('positions');
$table->ipAddress('visitor');
$table->macAddress('device');
$table->point('position');
$table->uuid('id');
```

MIra todos los tipos en la [official documentation]([Database: Migrations - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/master/migrations#creating-columns)).

### Default Timestamp

Mientras creamos las migraciones, puedes usar la columna`timestamp()` con la opción `useCurrent()` y `useCurrentOnUpdate()`, esto colocará el  `CURRENT_TIMESTAMP` como valor por defecto

```php
$table->timestamp('created_at')->useCurrent();
$table->timestamp('updated_at')->useCurrentOnUpdate();
```

### Migration Status

Si necesitas revisar qué migraciones ya han sido ejecutadas o no, no hay necesidad de mirar en la base de datos la tabla de migraciones,  solo necesitas el comando `php artisan migrate:status`

Ejemplo:

```
Migration name .......................................................................... Batch / Status  
2014_10_12_000000_create_users_table ........................................................... [1] Ran  
2014_10_12_100000_create_password_resets_table ................................................. [1] Ran  
2019_08_19_000000_create_failed_jobs_table ..................................................... [1] Ran    
```

### Create Migration with Spaces

Cuando ejectuamos el comando `make:migration`no necesariamente tienes que usar el guión bajo `_`  para dividirlas palabras. En vez de eso simplemente coloca el nombre entre doble commilas.

```php
// Esto funciona
php artisan make:migration create_transactions_table
// But this works too
php artisan make:migration "create transactions table"
```

⭐Aportación de: [Steve O on Twitter](https://twitter.com/stephenoldham/status/1353647972991578120)

### Create Column after Another Column

_Aviso: Solo en MySQL

Si estas añadiendo una nueva columa a una tabla ya existente, no necesariamente tiene que convertirse en la ultima en la lista. Puedes especificar despues de qué columa deberia ser creada

```php
Schema::table('users', function (Blueprint $table) {
    $table->string('phone')->after('email');
});
```

Si solo quieres que tu columna sea la prima en tu tabla, entonces usa el método `first()`

```php
Schema::table('users', function (Blueprint $table) {
    $table->string('uuid')->first();
});
```

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

Si quieres hacer una migracion para una tabla existente y quieres que laravel genere el Sachema::table() automaticamente, entonces añade "__in_xxx_table" o "_to_xxxx_table" y al final añade el parametro "--table".
`php artisan change_fields_products_table` gera una clase vacia  class

```php
class ChangeFieldsProductsTable extends Migration
{
    public function up()
    {
        //
    }
}
```

Pero al añadir `in_xxxxx_table` `php artisan make:migration change_fields_in_products_table` genera la clase con `Schema::table()`pre llenada.

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

Cuando colocamos el comando `php artisan migrate` en conjunto con el parametro `--pretend` nos retorna la query que será ejecutada en la termina. Esto es bastante interesante cuando se trata de debugear a nivel de base de datos.

```php
// Artisan command
php artisan migrate --pretend
```

⭐Aportación de [@zarpelon](https://github.com/zarpelon)

### Anonymous Migrations

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

⭐Aportación de [@nicksdot](https://twitter.com/nicksdot/status/1432340806275198978)

### ###You can add "comment" about a column inside your migrations

 Puedes añadir comentarios sobre una columna dentro de tus migraciones para guardar información sobre esa columna.

Si la base de datos es administrada por alguien mas que no es desarrollador, puede darle un vistazo a la estructura antes de realizar cualquier accion.

```php
$table->unsignedInteger('interval')
    ->index()
    ->comment('This column is used for indexing.')


```

 ⭐ Aportación de [@nicksdot](https://twitter.com/nicksdot/status/14323440806275198978)

### Checking For Table / Column Existence

Puedes revisar la existencia de una tabla o columna usando el método `hasTable` y `hasColumn`

```php
if (Schema::hasTable('users')) {
    // The "users" table exists...
}

if (Schema::hasColumn('users', 'email')) {
    // The "users" table exists and has an "email" column...
}
```

⭐ Aportación de [@dipeshsukhia](https://github.com/dipeshsukhia)

### Group Columns within an After Method

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

⭐ Aportación de[@ncosmeescobedo](https://twitter.com/cosmeescobedo/status/1512233993176973314)

### Add the column in the database table only if it's not present & can drop it if, its present

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

⭐ Aportación de [@iamharis010](https://twitter.com/iamharis010/status/1510579415163432961)

### Method to set the default value for current timestamp

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
