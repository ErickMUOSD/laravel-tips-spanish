## Validation

⬆️ [Menú principal](README.md#laravel-tips) ⬅️ [Anterior (Routing)](routing.md) ➡️ [Siguiente (Collections)](collections.md)

- [Image validation](#image-validation)
- [Add Values to the Form Request After Validation](#add-values-to-the-form-request-after-validation)
- [Access model binding in FormRequests](#access-model-binding-in-formrequests)
- [Rule which ensures the field under validation is required if another field is accepted](#rule-which-ensures-the-field-under-validation-is-required-if-another-field-is-accepted)
- [Custom validation error messages](#custom-validation-error-messages)
- [Validate dates with "now" or "yesterday" words](#validate-dates-with-now-or-yesterday-words)
- [Validation Rule with Some Conditions](#validation-rule-with-some-conditions)
- [Change Default Validation Messages](#change-default-validation-messages)
- [Prepare for Validation](#prepare-for-validation)
- [Stop on First Validation Error](#stop-on-first-validation-error)
- [Throw 422 status code without using validate() or Form Request](#throw-422-status-code-without-using-validate-or-form-request)
- [Rules depending on some other conditions](#rules-depending-on-some-other-conditions)
- [With Rule::when() we can conditionally apply validation rules](#with-rulewhen-we-can-conditionally-apply-validation-rules)
- [Use this property in the request classes to stop the validation of the whole request attributes](#use-this-property-in-the-request-classes-to-stop-the-validation-of-the-whole-request-attributes)
- [Rule::unique doesn't take into the SoftDeletes Global Scope applied on the Model](#ruleunique-doesnt-take-into-the-softdeletes-global-scope-applied-on-the-model)
- [Validator::sometimes() method allows us to define when a validation rule should be applied](#validatorsometimes-method-allows-us-to-define-when-a-validation-rule-should-be-applied)
- [Array elements validation](#array-elements-validation)
- [Password::defaults method](#passworddefaults-method)
- [Form Requests for validation redirection](#form-requests-for-validation-redirection)
- [Mac validation rule](#mac-validation-rule)
- [Validate email with TLD domain required](#validate-email-with-tld-domain-required)
- [New array validation rule required_array_keys](#new-array-validation-rule-required_array_keys)
- [Position placeholder in validation messages](#position-placeholder-in-validation-messages)
- [Exclude validation value](#exclude-validation-value)

### Image validation

Mientras validamos imagenes, puedes especificar las dimensiones que necesites.

```php
['photo' => 'dimensions:max_width=4096,max_height=4096']
```

### Add Values to the Form Request After Validation

```php
class UpdatedBookRequest extends FormRequent
{
     public function validated()
     {
          return array_merge(parent::validated(), [
               'user_id' => Auth::user()->id,
          ]);
     }
}
```

### Access model binding in FormRequests

Cuando usamos FormRequest, siempre puedes acceder al modelo usnado la siguiente expresión.

Aquí un ejemplo:

```php
class CommunityController extends Controller
{
     // ...
     public function update(CommunityUpdateRequest $request, Community $community)
     {
          $community->update($request->validated());

          return to_route('communities.index')->withMessage('Community updated successfully.');
     }
     // ...
}

class CommunityUpdateRequest extends FormRequest
{
     // ...
     public function rules()
     {
          return [
               'name' => ['required', Rule::unique('communities', 'name')->ignore($this->community)],
               'description' => ['required', 'min:5'],
          ];
     }
     // ...
}
```

⭐ Aportación de  [@bhaidar](https://twitter.com/bhaidar/status/1574715518501666817) 

### Rule which ensures the field under validation is required if another field is accepted

Puedes utilizar la regla de validación `required_if_accepted`, la cual asegura que el campo en validación es requerido si otro campo es aceptado (un valor de yes, on, 1 o true).

```php
Validator::make([
     'is_company' => 'on',
     'company_name' => 'Apple',
], [
     'is_company' => 'required|boolean',
     'company_name' => 'required_if_accepted:is_company',
]);
```


⭐ Aportación de [@iamgurmandeep](https://twitter.com/iamgurmandeep/status/1583420332693749761)

### Custom validation error messages

Puedes personalizar mensajes de errores por campo, regla y lenguaje. Solo crea un lenguaje nuevo en el archivo   `resources/lang/xx/validation.php` con la apropiada estructura.
```php
'custom' => [
     'email' => [
        'required' => 'We need to know your e-mail address!',
     ],
],
```

### Validate dates with "now" or "yesterday" words

Puedes validar fechas con reglas before/after y pasar varios parametros   como:  `tomorrow`, `now`, `yesterday`. Ejemplo:  `'start_date' => 'after:now'`.  Usa el método strtotime() por debajo.

```php
$rules = [
    'start_date' => 'after:tomorrow',
    'end_date' => 'after:start_date'
];
```

### Validation Rule with Some Conditions

SI tus reglas de validación dependen de alguna condición, puedes modificar las reglas añadiendo  `withValidator()` a tu clase FormRequest y especificar tu  lógica, por ejemplo si quieres añadir algunas reglas de validaciones para algun rol de usuario.

```php
use Illuminate\Validation\Validator;
class StoreBlogCategoryRequest extends FormRequest {
    public function withValidator(Validator $validator) {
        if (auth()->user()->is_admin) {
            $validator->addRules(['some_secret_password' => 'required']);
        }
    }
}
```

### Change Default Validation Messages

Si quieres cambiar los mensajes de error por defecto para un específico campo y una regla de validación específica, solo añade a tu clase FormRequest el méotodo `messages()` .

```php
class StoreUserRequest extends FormRequest
{
    public function rules()
    {
        return ['name' => 'required'];
    }

    public function messages()
    {
        return ['name.required' => 'User name should be real name'];
    }
}
```

### Prepare for Validation

Si quieres modificar algunso campos antes de la Vlidación de Laravel, es decir, preparar el campo para cierta lógica, añade a la clase `FormRequest` el método `prepareForValidation()`.
```php
protected function prepareForValidation()
{
    $this->merge([
        'slug' => Illuminate\Support\Str::slug($this->slug),
    ]);
}
```

### Stop on First Validation Error


Por defecto, los errores de validación serán retornados en una lista,. Pero si quieres parar el proceso de validación después del primer error sobre un campo específico usa la regla de validación `bail`:

```php
$request->validate([
    'title' => 'bail|required|unique:posts|max:255',
    'body' => 'required',
]);
```


En la clase `FormRequest` puedes colocar la propiedad `stopOnFirstFailure` en `true` para parar la validaicón de todos los campos:

```php
protected $stopOnFirstFailure = true;
```

### Throw 422 status code without using validate() or Form Request

Si no quieres usar validate() o un FomrRequest, pero aun necesitas lanzar errores con  el mismo status 422 y un envíar el error estructuradamente , puedes hacerlo manualmente con  `throw ValidationException::withMessages()`

```php
if (! $user || ! Hash::check($request->password, $user->password)) {
    throw ValidationException::withMessages([
        'email' => ['The provided credentials are incorrect.'],
    ]);
}
```

### Rules depending on some other conditions

Si tus reglas son dinámicas y dependen de otras condiciones, puedes crear el arreglo de reglas en la marcha.

```php
    public function store(Request $request)
    {
        $validationArray = [
            'title' => 'required',
            'company' => 'required',
            'logo' => 'file|max:2048',
            'location' => 'required',
            'apply_link' => 'required|url',
            'content' => 'required',
            'payment_method_id' => 'required'
        ];

        if (!Auth::check()) {
            $validationArray = array_merge($validationArray, [
                'email' => 'required|email|unique:users',
                'password' => 'required|confirmed|min:5',
                'name' => 'required'
            ]);
        }
        //
    }
```

### With Rule::when() we can conditionally apply validation rules

Gracias a la regla `Rule::when()` podemos condicionalmente aplicar reglas de validación en Laravel.

En este ejemplo validamos el valor del campo voto solo si el usuario tiene permisos para participar.
```php
use Illuminate\Validation\Rule;

public function rules()
{
    return [
        'vote' => Rule::when($user->can('vote', $post), 'required|int|between:1,5'),
    ]
}
```

⭐ Aportación de [@cerbero90](https://twitter.com/cerbero90/status/1434426076198014976)

### Use this property in the request classes to stop the validation of the whole request attributes

Utiliza esta propiedad en las clases de solicitud para detener la validación de todos los atributos de la solicitud.

Pista Directa

Esto es diferente de la regla `Bail`, que detiene la validación de un solo atributo si una de sus reglas no se cumple.
```php
/**
 * Indica si el validator debería parar todas las validaciones cuando falla una sola regla.
 */
protected $stopOnFirstFailure = true;
```

⭐ Aportación de [@Sala7JR](https://twitter.com/Sala7JR/status/1436172331198603270) 

### Rule::unique doesn't take into the SoftDeletes Global Scope applied on the Model

Es extraño que `Rule::unique` no tenga en cuenta el alcance global de SoftDeletes aplicado al modelo de forma predeterminada.

Sin embargo, el método `withoutTrashed()` está disponible.
```php
Rule::unique('users', 'email')->withoutTrashed();
```

Tip given by [@Zubairmohsin33](https://twitter.com/Zubairmohsin33/status/1438490197956702209)
⭐ Aportación de [@Zubairmohsin33](https://twitter.com/Zubairmohsin33/status/1438490197956702209)

### Validator::sometimes() method allows us to define when a validation rule should be applied

El método  `Validator::sometimes()`  nos permite definir cuando una regla de validación debe ser aplicada, basada en los parametros pasados.

```php
$data = [
    'coupon' => 'PIZZA_PARTY',
    'items' => [
        [
            'id' => 1,
            'quantity' => 2
        ],
        [
            'id' => 2,
            'quantity' => 2,
        ],
    ],
];

$validator = Validator::make($data, [
    'coupon' => 'exists:coupons,name',
    'items' => 'required|array',
    'items.*.id' => 'required|int',
    'items.*.quantity' => 'required|int',
]);

$validator->sometimes('coupon', 'prohibited', function (Fluent $data) {
    return collect($data->items)->sum('quantity') < 5;
});

// throws a ValidationException as the quantity provided is not enough
$validator->validate();
```

⭐ Aportación de [@cerbero90](https://twitter.com/cerbero90/status/1440226037972013056)

### Array elements validation

Si quieres validar los elementos dentro de cada posición de  un arreglo usa la palabra `*` en las reglas:

```php
// say you have this array
// array in request 'user_info'
$request->validated()->user_info = [
    [
        'name' => 'Qasim',
        'age' => 26,
    ],
    [
        'name' => 'Ahmed',
        'age' => 23,
    ],
];

// Rule
$rules = [
    'user_info.*.name' => ['required', 'alpha'],
    'user_info.*.age' => ['required', 'numeric'],
];
```


⭐ Aportación de [HydroMoon](https://github.com/HydroMoon)

### Password::defaults method


Puedes especificar algunas reglas cuando validamos contraseñas usando el método ` Password::defaults` . Incluye opciones para letras, simbolos y más.
```php
class AppServiceProvider
{
    public function boot(): void
    {
        Password::defaults(function () {
            return Password::min(12)
                ->letters()
                ->numbers()
                ->symbols()
                ->mixedCase()
                ->uncompromised();
        })
    }
}

request()->validate([
    ['password' => ['required', Password::defaults()]]
])
```


⭐ Aportación de [@mattkingshott](https://twitter.com/mattkingshott/status/1463190613260603395)

### Form Requests for validation redirection

Cuando usamos FormRequest para validaciones, por defecto los errores de validación serán redireccionados a la página anterior, pero puedes sobrescribirla. SOlo define la propiedad  `$redirect` o `$redirectRoute`.

```php
// La URI que el usuario debería ser redireccionado si la validación falla
protected $redirect = '/dashboard';

// La ruta que los uusarios deberían ser redireccionados si la validación falla
protected $redirectRoute = 'dashboard';
```

### Mac validation rule

En Laravel 8.77 se han agregado reglas de validación para direcciones MAC
.
```php
$trans = $this->getIlluminateArrayTranslator();
$validator = new Validator($trans, ['mac' => '01-23-45-67-89-ab'], ['mac' => 'mac_address']);
$this->assertTrue($validator->passes());
```

⭐ Aportación de [@Teacoders](https://twitter.com/Teacoders/status/1475500006673027072)

### Validate email with TLD domain required

Por defecto, las reglas de validación  para los email aceptarán un email sin un tld dominio,

Pero si quieres asegurarte que el email tenga un tld dominio, usa la regla  `email:filter` .

```php
[
    'email' => 'required|email', // before
    'email' => 'required|email:filter', // after
],
```

⭐ Aportación de [@Chris1904](https://laracasts.com/discuss/channels/general-discussion/laravel-58-override-email-validation-use-57-rules?replyId=645613)

### New array validation rule required_array_keys

Laravel 8.82 añade una regla de validación `required_array_keys`  que verifica que todos las llaves(keys) de un arreglo existan.

```php
$data = [
    'baz' => [
        'foo' => 'bar',
        'fee' => 'faa',
        'laa' => 'lee'
    ],
];

$rules = [
    'baz' => [
        'array',
        'required_array_keys:foo,fee,laa',
    ],
];

$validator = Validator::make($data, $rules);
$validator->passes(); // true
```

Datos que estén en la validación pero no se encuentren en el arreglo aunque todos los demás sí, fallarían la validaición:

```php
$data = [
    'baz' => [
        'foo' => 'bar',
        'fee' => 'faa',
    ],
];

$rules = [
    'baz' => [
        'array',
        'required_array_keys:foo,fee,laa',
    ],
];

$validator = Validator::make($data, $rules);
$validator->passes(); // false
```

⭐ Aportación de [@AshAllenDesign](https://twitter.com/AshAllenDesign/status/1488853052765478914) 

### Position placeholder in validation messages

En Laravel  9 puedes usar el  atributo `:position`  en los mensjaes de validación si esque estas trabajando con arreglos.

En este ejemplo el resultado sería "Porfavor provee una cantidad para el precio #2".

```php
class CreateProductRequest extends FormRequest
{
    public function rules(): array
    {
        return  [
            'title' => ['required', 'string'];
            'description' => ['nullable', 'sometimes', 'string'],
            'prices' => ['required', 'array'],
            'prices.*.amount' => ['required', 'numeric'],
            'prices.*.expired_at' => ['required', 'date'],
        ];
    }

    public function messages(): array
    {
        'prices.*.amount.required' => 'Please provide an amount for price #:position'
    }
}
```

⭐ Aportación de  [@mmartin_joo](https://twitter.com/mmartin_joo/status/1502299053635235842)

### Exclude validation value

Cuando necesitas validar un campo, pero en realidad no lo necesitas más adelante, por ejemplo  el campo marcado 'Acepto terminos y condiciones', asegurate de usar la regla  `exclude`. De esta menera el método validated no lo regresará.

```php
class StoreRequest extends FormRequest
{
    public function rules(): array
    {
        return [
            'name' => 'required|string',
            'email_address' => 'required|email',
            'terms_and_conditions' => 'required|accepted|exclude',
        ];
    }
```

```php
class RegistrationController extends Controller
{
    public function store(StoreRequest $request)
    {
        $payload = $request->validated(); // only name and email

        $user = User::create($payload);

        Auth::login($user);

        return redirect()->route('dashboard');
    }
```

⭐ Aportación de  [@mattkingshott](https://twitter.com/mattkingshott/status/1518590652682063873)
