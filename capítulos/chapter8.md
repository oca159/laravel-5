# Model Factories
Los model factories son una excelente forma de poblar nuestra base de datos con datos de prueba generados automáticamente. Laravel en su versión 5.1 incorpora este nuevo componente por defecto, en versiones anteriores a Laravel 5.1 era necesario agregar el componente faker en nuestro ****composer.json**** y realizar el proceso de manera manual en los archivos seeders, para más información sobre estre proceso puedes visitar el link de github del componente [Faker](https://github.com/fzaninotto/Faker).

Los model Factories en realidad también trabajan con el componente Faker, esto lo podemos confirmar si miramos nuestro **composer.json**, sin embargo, nos ofrecen una manera más elegante y ordenada de trabajar.

Laravel 5.1 trae un ejemplo de como usar este nuevo componente, lo podemos encontrar en el archivo  **database/factories/ModelFactory.php**.

El método `$factory->define()` regresa un array con los datos del modelo que se va a poblar, recibe como primer parámetro el modelo con el que deseamos trabajar y como segundo parámetro una función que recibe como parámetro un objeto **$faker**.

Ejemplo:
```php 
$factory->define(App\User::class, function ($faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->email,
        'password' => str_random(10),
        'remember_token' => str_random(10),
    ];
}); 
```

El método `$factory->defineAs()` regresa un array con los datos del modelo que se va a poblar, recibe como primer parámetro el modelo con el que deseamos trabajar, como segundo parámetro un tipo especifico de poblado y como tercer parámetro una función que recibe como parámetro un objeto **$faker**.

Ejemplo:
```php
// Creamos un model factory para poblar usuarios de tipo administrador
$factory->defineAs(App\User::class, 'administrador', function ($faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->email,
        'password' => str_random(10),
        'type' => 'administrador',
        'remember_token' => str_random(10),
    ];
});

// Creamos un model factory para poblar usuarios de tipo encargado
$factory->defineAs(App\User::class, 'encargado', function ($faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->email,
        'password' => str_random(10),
        'type' => 'encargado',
        'remember_token' => str_random(10),
    ];
});
```


En el ejemplo de arriba hemos creado dos tipos de poblado para el modelo **User**, uno será para poblar usuarios de tipo "administrador" y otro para usuarios de tipo "encargado".

Una vez creados los Model Factories, debemos ir al archivo **database/seeds/DatabaseSeeder.php** y ejecutar el poblado dentro del método `run` como en el siguiente ejemplo:

```php
public function run()
{
        Model::unguard();
        factory('Curso\User', 50)->create();
        factory('Curso\User','administrador',1)->create();        
        // $this->call('UserTableSeeder');
        Model::reguard();
}
```

El objeto `factory` recibe como parámetros el nombre del modelo, el tipo de poblado como parámetro opcional y el número de registros que deseamos crear. Con el método `create` realizamos el poblado de datos.

Ejemplos:

```php
factory('Curso\User',100)->create();

// Opcionalmente podemos agregar el tipo de poblado
factory('Curso\User', 'administrador',100)->create();
```










