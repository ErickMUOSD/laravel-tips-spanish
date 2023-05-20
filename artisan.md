## Artisan

⬆️ [Menú principal](README.md#laravel-tips) ⬅️ [Anterior (Mail)](mail.md) ➡️ [Siguiente (Factories)](factories.md)

- [Artisan command parameters](#artisan-command-parameters)
- [Execute a Closure after command runs without errors or has any errors](#execute-a-closure-after-command-runs-without-errors-or-has-any-errors)
- [Run artisan commands on specific environments](#run-artisan-commands-on-specific-environments)
- [Maintenance Mode](#maintenance-mode)
- [Artisan command help](#artisan-command-help)
- [Exact Laravel version](#exact-laravel-version)
- [Launch Artisan command from anywhere](#launch-artisan-command-from-anywhere)
- [Hide your custom command](#hide-your-custom-command)
- [Skip method](#Skip method)

### Artisan command parameters

Cuando creamos comandos, podemos pedir la entrada de datos de diferentes formas: `$this->confirm()`, `$this->anticipate()`, `$this->choice()`.
```php
// Si o no?
if ($this->confirm('Do you wish to continue?')) {
    //
}

// Preguntas aviertas con opciones autocompletado
$name = $this->anticipate('What is your name?', ['Taylor', 'Dayle']);

// Una de las opciones listadas con un index por defecto
$name = $this->choice('What is your name?', ['Taylor', 'Dayle'], $defaultIndex);
```

### Execute a Closure after command runs without errors or has any errors

Con Laravel Scheduler puedes ejecutar una funcion cuando un comando  sin errores con el método `onSuccess` y también  cuando existen errores con el método `onFailure()`.
```php
protected function schedule(Schedule $schedule)
{
    $schedule->command('newsletter:send')
        ->mondays()
        ->onSuccess(fn () => resolve(SendNewsletterSlackNotification::class)->handle(true))
        ->onFailure(fn () => resolve(SendNewsletterSlackNotification::class)->handle(false));
}
```


⭐ Aportación de [@wendell_adriel](https://twitter.com/wendell_adriel)

### Run artisan commands on specific environments


Toma control de tus comandos programados en Laravel y correlos en entornos especificos para más flexibilidad. 
```php
$schedule->command('reports:send')
    ->daily()
    ->environments(['production', 'staging']);
```


⭐ Aportación de  [@LaraShout](https://twitter.com/LaraShout)

### Maintenance Mode

Si quieres habilitar el modo mantenimiento en tú página, ejectuta el cómando `down` .
```bash
php artisan down
```

Las personas que visiten la página recibirán el status 503.

Cosidera algunos parametros para esto en Laravel 8:

- La ruta que el usuario debería ser redirigido
- La vista que debería ser rendereada
- Frase secreta para saltarse el modo  mantenimiento
- Número de intetos por cada x segundos

```bash
php artisan down --redirect="/" --render="errors::503" --secret="1630542a-246b-4b66-afa1-dd72a4c43515" --retry=60
```

Antes de Laravel 8:
- Los mensajes deberían ser mostrados
- Número de intetos x segundos
- Permir el acceso a alguna dirección IP
```bash
php artisan down --message="Upgrading Database" --retry=60 --allow=127.0.0.1
```

Cuando quieras quitar el modo mantenimiento solo coloca el comando:

```bash
php artisan up
```

### Artisan command help

Para revisar las opciones disponibles de cada comando. Corre el comando con el parametro `--help`. Por ejemplo `php artisan make:model --help`  y ver cuantas opciones tienes:
```
Options:
  -a, --all             Generate a migration, seeder, factory, policy, resource controller, and form request classes for the model
  -c, --controller      Create a new controller for the model
  -f, --factory         Create a new factory for the model
      --force           Create the class even if the model already exists
  -m, --migration       Create a new migration file for the model
      --morph-pivot     Indicates if the generated model should be a custom polymorphic intermediate table model
      --policy          Create a new policy for the model
  -s, --seed            Create a new seeder for the model
  -p, --pivot           Indicates if the generated model should be a custom intermediate table model
  -r, --resource        Indicates if the generated controller should be a resource controller
      --api             Indicates if the generated controller should be an API resource controller
  -R, --requests        Create new form request classes and use them in the resource controller
      --test            Generate an accompanying PHPUnit test for the Model
      --pest            Generate an accompanying Pest test for the Model
  -h, --help            Display help for the given command. When no command is given display help for the list command
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi|--no-ansi  Force (or disable --no-ansi) ANSI output
  -n, --no-interaction  Do not ask any interactive question
      --env[=ENV]       The environment the command should run under
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug
```

### Exact Laravel version

Para saber la version de Laraver que tienes en tu aplicación, solo corre el comando:
`php artisan --version`

### Launch Artisan command from anywhere

Si tienes un comando, puedes lanzarlo no solo de la Terminal, sino también desde cualquier parte del código si es necesario con algunos parametros. Para esto usa el comando `Artisan::call()`:

```php
Route::get('/foo', function () {
    $exitCode = Artisan::call('email:send', [
        'user' => 1, '--queue' => 'default'
    ]);

    //
});
```

### Hide your custom command

Si no quieres mostrar algun comando en la  lista de comandos que lanzas a través de  `php artisan`   solo debes colocar `True` en el atributo `Hidden`
```php
class SendMail extends Command
{
    protected $signature = 'send:mail';
    protected $hidden = true;
}
```

Así no verás `send:mail`  en los comandos disponibles.

⭐ Aportación de  [@sky_0xs](https://twitter.com/sky_0xs/status/1487921500023832579)

### Skip method

Puedes saltar/evitar la ejeción de  tus comandos programados colocando el método `skip()` retornando  alguna fecha para ser excluida .
```php
$schedule->command('emails:send')->daily()->skip(function () {
    return Calendar::isHoliday();
});
```

⭐ Aportación de  [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1494503181438492675)