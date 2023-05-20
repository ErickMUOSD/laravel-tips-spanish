## Mail

⬆️ [Meńu principal](README.md#laravel-tips) ⬅️ [Anterior (Auth)](auth.md) ➡️ [Siguiente (Artisan)](artisan.md)

- [Testing email into laravel.log](#testing-email-into-laravellog)
- [You don’t have to store files to use them as email attachments in Laravel](#you-dont-have-to-store-files-to-use-them-as-email-attachments-in-laravel)
- [Preview Mailables](#preview-mailables)
- [Preview Mail without Mailables](#preview-mail-without-mailables)
- [Default Email Subject in Laravel Notifications](#default-email-subject-in-laravel-notifications)
- [Send Notifications to Anyone](#send-notifications-to-anyone)
- [Set conditional object properties](#set-conditional-object-properties)

### Testing email into laravel.log

Si necesitas testear/probar el contenido de los emails en tu aplicación pero no quieres configurar los servicios como Maligun, Mailtrap, etc. Solo necesitas colocar  en el .env `MAIL_DRIVER=log`  para que todos los emails sean guaradados en  `storage/logs/laravel.log`, en vez de enviarlo.

### You don’t have to store files to use them as email attachments in Laravel

Simplemente usa el método `attachData`  para añadir archivos a Emails.

Te presentamos el siguiente ejemplo usnado la class Mailable.
```php
public function build()
{
     return $this->subject('Inquiry')
          ->to('example@example.com')
          ->markdown('email.inquiry')
          ->attachData(
               $this->file,
               $this->file->getClientOriginalName(),
          );
}
```

⭐ Aportación de   [@ecrmnn](https://twitter.com/ecrmnn/status/1570449885664808961)

### Preview Mailables

Si tu usas Mailables para enviar emails, puedes previsualizar el resultado sin  enviarlos a algun servicio, mandandolos  al navegador. Para esto necesitas retornar el email como resultado. 

```php
Route::get('/mailable', function () {
    $invoice = App\Invoice::find(1);
    return new App\Mail\InvoicePaid($invoice);
});
```

### Preview Mail without Mailables

También puedes previsualizar tu correo electrónico sin Mailables. Por ejemplo, al crear una notificación, puedes especificar el formato markdown que se utilizará para tu notificación por correo electrónico.
```php
use Illuminate\Notifications\Messages\MailMessage;

Route::get('/mailable', function () {
    $invoice = App\Invoice::find(1);
    return (new MailMessage)->markdown('emails.invoice-paid', compact('invoice'));
});
```

De hecho puedes usar otros métodos provistos por  el objeto `MailSessage` como  `view` y otros.

⭐ Aportación de [@raditzfarhan](https://github.com/raditzfarhan)
### Default Email Subject in Laravel Notifications

Si necesitas enviar Laravel notificaciones y omites especificar el parametro `toMail()`, por defecto el asunto(subject) será el nombre de la clase.

Por ejemplo tienes:
```php
class UserRegistrationEmail extends Notification {
    //
}
```

Entonces recivirás un email con el aunto  **User Registration Email**.

### Send Notifications to Anyone

También puedes enviar Laravel Notificaciones no solo a ciertos usuarios sino notificar a cualquiera que necesites con el método  `Notification::route()` con las notificaciones "on-demand":
```php
Notification::route('mail', 'taylor@example.com')
        ->route('nexmo', '5555555555')
        ->route('slack', 'https://hooks.slack.com/services/...')
        ->notify(new InvoicePaid($invoice));
```

### Set conditional object properties

Puedes usar el método  `when()` o `unless()` en tus notificaciones MailMessage para colocar propiedades de manera condicionales.

```php
class InvoicePaid extends Notification
{
    public function toMail(User $user)
    {
        return (new MailMessage)
            ->success()
            ->line('We\'ve received your payment')
            ->when($user->isOnMonthlyPaymentPlan(), function (MailMessage $message) {
                $message->action('Save 20% by paying yearly', route('account.billing'));
            })
            ->line('Thank you for using Unlock.sh');
    }
}
```

Usa los métodos `when()` o `unless()` en tus propias clases usando el Trait  `Illuminate\Support\Traits\Conditionable` .

⭐ Aportación de  [@Philo01](https://twitter.com/Philo01/status/1503302749525528582)
