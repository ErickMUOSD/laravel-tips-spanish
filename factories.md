## Factories

⬆️ [Go to main menu](README.md#laravel-tips) ⬅️ [Previous (Artisan)](artisan.md) ➡️ [Next (Log and debug)](log-and-debug.md)

- [Factory callbacks](#factory-callbacks)
- [Generate Images with Seeds/Factories](#generate-images-with-seedsfactories)
- [Override values and apply custom login to them](#override-values-and-apply-custom-login-to-them)
- [Using factories with relationships](#using-factories-with-relationships)
- [Create models without dispatching any events](#create-models-without-dispatching-any-events)
- [Useful for() method](#useful-for-method)
- [Run factories without dispatching events](#run-factories-without-dispatching-events)
- [Specify dependencies in the run() method](#specify-dependencies-in-the-run-method)

### Factory callbacks

Mientras usamos Factories para insertar datos, podemos añadirle un Callback a las funciones para realizar alguna acción después de que los registros son insertados.

```php
$factory->afterCreating(App\User::class, function ($user, $faker) {
    $user->accounts()->save(factory(App\Account::class)->make());
});
```

### Generate Images with Seeds/Factories

¿Sabías que la clase Faker puede generar no solo  texto sino imagenes?
En este ejemplo el atributo avatar insertamos una imagen 50x50:
```php
$factory->define(User::class, function (Faker $faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->unique()->safeEmail,
        'email_verified_at' => now(),
        'password' => bcrypt('password'),
        'remember_token' => Str::random(10),
        'avatar' => $faker->image(storage_path('images'), 50, 50)
    ];
});
```

### Override values and apply custom login to them

Cuando creamos registros con Factories, podemos usar la clase Sequence para sobrescribir valores y así generar diferente lógica.

```php
$users = User::factory()
                ->count(10)
                ->state(new Sequence(
                    ['admin' => 'Y'],
                    ['admin' => 'N'],
                ))
                ->create();
```

### Using factories with relationships

Laravel provee soporte  para aplicar métodos mágicos a las relaciones usando Factories.

```php
// factories con métodos mágicos en las relaciones
User::factory()->hasPosts(3)->create();

// en vez de esto
User::factory()->has(Post::factory()->count(3))->create();
```

⭐ Aportación de  [@oliverds\_](https://twitter.com/oliverds_/status/1441447356323430402)

### Create models without dispatching any events

Para *evitar* disparar algun evento sobre el modelo cuando utilizamos métodos como:  `update, create`  es necesario usar métodos como: `updateQuietly, createQuietly`. 

```php
Post::factory()->createOneQuietly();

Post::factory()->count(3)->createQuietly();

Post::factory()->createManyQuietly([
    ['message' => 'A new comment'],
    ['message' => 'Another new comment'],
]);
```

### Useful for() method

En Laravel el método `for()` es bastante util para crear relaciones tipo `belongsTo()`.

```php
public function run()
{
    Product::factory()
        ->count(3)
        ->for(Category::factory()->create())
        ->create();
}
```

⭐ Aportación de [@mmartin_joo](https://twitter.com/mmartin_joo/status/1461002439629361158)

### Run factories without dispatching events

Si deseas crear registros masviamente sin lanzar eventos usando Factories necesitamos envolver nuestro código dentro de el método `withoutEvents

```php
$posts = Post::withoutEvents(function () {
    return Post::factory()->count(50)->create();
});
```

⭐ Aportación de [@TheLaravelDev](https://twitter.com/TheLaravelDev/status/1510965402666676227)

### Specify dependencies in the run() method

Puedes especificar algunas dependecias en el método `run()` del seeder.

```php
class DatabaseSeeder extends Seeder
{
    public function run()
    {
        $user = User::factory()->create();

        $this->callWith(EventSeeder::class, [
            'user' => $user
        ]);
    }
}
```

```php
class EventSeeder extends Seeder
{
    public function run(User $user)
    {
        Event::factory()
            ->when($user, fn($f) => $f->for('user'))
            ->for(Program::factory())
            ->create();
    }
}
```

⭐ Aportación de  [@justsanjit](https://twitter.com/justsanjit/status/1514428294418079746)
