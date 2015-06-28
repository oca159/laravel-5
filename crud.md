#Creacion del CRUD con Laravel desde 0

Este anexo del libro de Laravel 5 esta pensado para resumir el contenido de una forma aplicada, de modo que se puedan ver en conjunto todos los conocimientos adquiridos durante el curso.

Se explicara el proceso para dar altas, bajas, cambios y consultas de la tabla **pasteles** o [CRUD](https://es.wikipedia.org/wiki/CRUD) que significa **C**reate, **R**ead, **U**pdate and **D**elete por sus siglas en ingles.

Primero retomaremos las [migraciones y los seeders](chapter5.md), creando la migracion y un pequeño seeder para poblar nuestra BD.

Para crear la migracion con la plantilla basica usaremos el comando 

```
	php artisan make:migration crear_tabla_pasteles --create=pasteles
```

Ahora dentro de la migracion vamos a definir la estructura de la tabla que tendra solo cuatro campos que seran: id, nombre, sabor y timestamps(esto en BD es igual a created_at y updated_at).

Dando un resultado como lo siguiente:

```
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CrearTablaPasteles extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('pasteles', function (Blueprint $table) {
            $table->increments('id');
            $table->string('nombre', 60);
            $table->enum('sabor', ['chocolate','vainilla','cheesecake']);
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::drop('pasteles');
    }
}
```

Ahora crearemos el seeder con el comando:

```
	php artisan make:seeder PastelesSeeder
```

y vamos a usar el componente [Faker](https://github.com/fzaninotto/Faker) para crear 50 pasteles de forma automatica, el archivo final quedara de la siguiente forma, recuerden que para usar [Faker](https://github.com/fzaninotto/Faker) es necesario importar la clase con la intruccion **use**:

```
<?php

use Illuminate\Database\Seeder;
use Faker\Factory as Faker;

class PastelesSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        $faker = Faker::create();
        for ($i=0; $i < 50; $i++) { 
            \DB::table('pasteles')->insert(array(
                   'nombre' => 'Pastel ' . $faker->firstNameFemale . ' ' . $faker->lastName,
                   'sabor'  => $faker->randomElement(['chocolate','vainilla','cheesecake']),
                   'created_at' => date('Y-m-d H:m:s'),
                   'updated_at' => date('Y-m-d H:m:s')
            ));
        }
    }
}
```

Con esto ya tendremos la estructura de la tabla y un seeder para poblar, con esto ahora usaremos el siguiente comando para crear la tabla en la BD y llenarla:

```
	php artisan migrate --seed
```

Ahora vamos a pasar a crear el [Modelo](chapter9.md), el cual nos va a servir para mapear la tabla de la BD a una clase de Laravel como lo vimos en el [capitulo 9](chapter9.modo). Vamos a usar el comando:

```
	php artisan make:model Pastel
```

Laravel nos recomienda seguir las convenciones para facilitarnos el trabajo, por lo que las tablas de la Base de datos deben encontrarse en notacion **underscore** y en **plural**, y los modelos deben encontrarse en notacion **UpperCamelCase** y en **singular**.

Con esto Laravel nos creara el archivo Pastel.php en la carpeta ```app/```, si seguimos las convenciones de Laravel por defecto esto seria lo unico necesario, pero debido a que nuestro lenguaje no convierte las palabras a plural de la misma forma que el ingles entonces debemos definir al modelo con que tabla va a estar trabajando. Agregaremos dentro del modelo el atributo ```protected $table = 'pasteles';```, y eso seria todo para el modelo, quedando de la siguiente manera:

```
<?php

namespace Curso;

use Illuminate\Database\Eloquent\Model;

class Pastel extends Model
{
    protected $table = 'pasteles';
}
```

Ya tenemos listo nuestro manejo de datos, ahora vamos a proceder a crear nuestras vistas para que desde el navegador podamos mandar la informacion y un controlador para poder definir nuestras operaciones, ademas de especificar las rutas de nuestro sistema.

Para comenzar vamos a crear nuestro controlador con el comando:

```
	php artisan make:controller PastelesController
```

Laravel automaticamente crea un archivo dentro de la ruta ```app/Http/Controllers``` con el nombre que especificamos y dentro de el las funciones para los metodos: **index**, **create**, **store**, **show**, **edit**, **update**, **delete**. Estos metodos los vamos a definir mas adelante, por el momento vamos a crear en nuestro archivo de rutas un grupo de rutas asociadas a cada uno de los metodos del controlador que acabamos de crear, esto se hace con una ruta de tipo **Resource** o recurso, modificando el archivo routes agregando el siguiente codigo:

```
	Route::resource('pasteles', 'PastelesController');
```

Con esto ya tendremos nuestro controlador(aun vacio) y nuestras rutas del sistema, lo cual podemos verificar con el comando:

```
	php artisan route:list
```

