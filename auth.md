## Auth

⬆️ [Menú principal](README.md#laravel-tips) ⬅️ [Anterior (Collections)](collections.md) ➡️ [Siguiente(Mail)](mail.md)

- [Check Multiple Permissions at Once](#check-multiple-permissions-at-once)
- [Authenticate users with more options](#authenticate-users-with-more-options)
- [More Events on User Registration](#more-events-on-user-registration)
- [Did you know about Auth::once()?](#did-you-know-about-authonce)
- [Change API Token on users password update](#change-api-token-on-users-password-update)
- [Override Permissions for Super Admin](#override-permissions-for-super-admin)

### Check Multiple Permissions at Once

En complemento a la directiva `@can` ¿Sabías que puedes checar multiples permisos en una sola linea de código con  la directiva `@canany`? 

```blade
@canany(['update', 'view', 'delete'], $post)
    // El actual usuario puede actualizar, mirar o borrar el post
@elsecanany(['create'], \App\Post::class)
    // EL actual usuario puede crear un post
@endcanany
```

### Authenticate users with more options

Si solo quieres  autenticar usuarios cumplan con cierta condición,  porejemplo  que en la columna "activated_at" no sea nulo,  es tan simple como pasar un argumento extra a `Auth::attempt()`.

No se necesitan middleware complejos o scopes globales.

```php
Auth::attempt(
    [
        ...$request->only('email', 'password'),
        fn ($query) => $query->whereNotNull('activated_at')
    ],
    $this->boolean('remember')
);
```

⭐Aportación de [@LukeDowning19](https://twitter.com/LukeDowning19)

### More Events on User Registration

¿Necesitas realizar algunas acciones depués de  la registración de un usuario? Dale un vistazo a `app/Providers/EventServiceProvider.php` y añade mas clases Listeners para  despues en  esas clases implementar el método `handle()`con el objeto `$event->user`

```php
class EventServiceProvider extends ServiceProvider
{
    protected $listen = [
        Registered::class => [
            SendEmailVerificationNotification::class,

            // You can add any Listener class here
            // With handle() method inside of that class
        ],
    ];
```

### Did you know about Auth::once()?

Puedes logear cualquier usuario  solo para **UNA PETICION**, usando el método `Auth::once()`. No hay necesidad de usar sessions o cookies lo cual significa que este método puede ser utíl cuando construimos una stateless API

```php
if (Auth::once($credentials)) {
    //
}
```

### Change API Token on users password update

Es conveniente cambiar el API token del usuario cuando su contraseña cambia.

En el modelo definimos:

```php
protected function password(): Attribute
{
    return Attribute::make(
            set: function ($value, $attributes) {
                $value = $value;
                $attributes['api_token'] = Str::random(100);
            }
        );
}
```

### Override Permissions for Super Admin

Si has definido tus puertas de acceso (Gates) pero quieres sobreescribir todos los permisos para el usuario SUPER ADMIN, para darle a ese superadministrador TODOS los permisos, puedes interceptar las puertas de acceso utilizando la declaración `Gate::before()` en el archivo `AuthServiceProvider.php`.

```php
// Interceptar cualquier Gate y checar si es super admin
Gate::before(function(user, ability) {
 if (user->is_super_admin == 1) {
 return true;
 }
});
// O si solo quiere algunos permisos del paquete
Gate::before(function(user, ability) {
 if (user->hasPermission('root')) {
 return true;
 }
});
```

Si deseas hacer algo en tu puerta de acceso (Gate) cuando no hay ningún usuario en absoluto, debes agregar un tipo de indicación (`type hint`) para `$user` permitiendo que sea `null`. Por ejemplo, si tienes un rol llamado "Anonymous" para tus usuarios no autenticados:

```php
Gate::before(function (?User $user, $ability) {
    if ($user === null) {
        $role = Role::findByName('Anonymous');
        return $role->hasPermissionTo($ability) ? true : null;
    }
    return $user->hasRole('Super Admin') ? true : null;
});
```
