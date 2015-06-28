#Creacion del CRUD con Laravel desde 0

Este anexo del libro de Laravel 5 esta pensado para resumir el contenido de una forma aplicada, de modo que se puedan ver en conjunto todos los conocimientos adquiridos durante el curso.

Se explicara el proceso para dar altas, bajas, cambios y consultas de la tabla **pasteles** o [CRUD](https://es.wikipedia.org/wiki/CRUD) que significa **C**reate, **R**ead, **U**pdate and **D**elete por sus siglas en ingles.

Primero retomaremos las [migraciones y los seeders](chapter6.md), creando la migracion y un pequeño seeder para poblar nuestra BD.

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

Ahora vamos a pasar a crear el [Modelo](capitulos/chapter9.md), el cual nos va a servir para mapear la tabla de la BD a una clase de Laravel como lo vimos en el [capitulo 9](capitulos/chapter9.md). Vamos a usar el comando:

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

Ahora vamos a terminar de llenar nuestro controlador, debemos importar la clase Pastel a nuestro controlador para poder hacer uso del Modelo y asi trabajar con la base de datos, esto con la ayuda de [Eloquent](capitulos/chapter9.md), como vimos durante el curso para crear un nuevo registro con Eloquent basta con definir una variable de un **new** Modelo, en este caso **Pastel**, segun se describe en el [capitulo 11](capitulos/chapter11.md) los metodos responden a una ruta en especifico de la ruta resoure que agregamos en **routes.php**, para este caso vamos a definir cada uno de ellos en orden:

####Index - Pagina de Inicio

Cuando entramos a una pagina principal de administracion se pueden ver en ocasiones la informacion que se encuentra en la BD y acceso a las operaciones basicas del CRUD, por lo cual debemos ser capaces de recibir en el index los registros de la BD. Dentro el metodo **index()**, vamos a usa Eloquent para obtener todos los pasteles y enviarlos a una vista que vamos a definir mas adelante, quedando de la siguiente forma:

```
	public function index()
    {
        $pasteles = Pastel::get();
        return view('pasteles.index')->with('pasteles', $pasteles);
    }
```

Eloquent nos facilita mucho las consultas a la BD y hace que sea portable nuestro codigo, en el metodo decimos que seleccione todos los pasteles y os envie a una vista llama **index** ubicada en la carpeta ```resoures/views/pasteles/```, como vimmos en el [capitulo 10](chapter10.md) las rutas en blade cambian la **/**(diagonal) por un **.**(punto), la funcion ```view('pasteles.index');``` toma como carpeta raiz a ```resources/views/``` por lo que no tenemos la necesidad de agregarlo en la ruta. Ademas se esta concatenando el metodo ```with('nombre', $var);``` que como **primer** parametro pide el nombre con el cual se va a poder usar una variable del lado de la vista, y como **segundo** parametro recibe la variable que se va a mandar a la vista.

####Create - Pagina de registro

Este metodo es muy sencillo puesto que solo va a devolver una vista sin ninguna variable ni uso de Eloquent, por lo cual queda de la siguiente manera:

```
	public function create()
    {
        return view('pasteles.create');
    }
```

####Store - Funcion de almacenamiento

Este metodo es donde despues de haber entrado a **create** se reciben los datos y se guardan en la base de datos, para poder recibir la informacion en este ejemplo vamos a usar la clase **Request** que significa peticion y es una clase que Laravel agrega por nosotros cuando creamos el controlador, vamos a pasar por parametro la peticion en el metodo definiendo que es una variable de la clase **Request** y despues de eso podemos recuperar por el nombre del campo del formulario(atributo **name**) la informacion enviada, entonces el metodo quedaria de la siguiente forma:

```
	public function store(Request $request)
    {
        $pastel = new Pastel;
        $pastel->nombre = $request->input('nombre');
        $pastel->sabor  = $request->input('sabor');

        $pastel->save();

        return redirect()->route('pasteles.index');
    }
```

En la funcion estamos creando una instancia de un nuevo Pastel y asignando los atributos de la clase que se llaman igual que los campos de la BD los valores del formulario con la variable request y el metodo ```input('name');``` que recibe como parametro el nombre del campo del formulario, para mas detalle revise la seccion del [Anexo A de HTML](HTML.md) que habla sobre los atributos de los formularios.

Despues de asignar los valores de la peticion a la variable ```$pastel```, se usa el metodo ```save();``` para que el modelo se encargue de guardar los datos en la BD y finalmente redireccionar al index con los metodos encadenados: ```redirect()->route('pasteles.index');```.

####Show - Pagina de descripcion

* Lo sentimos, seccion en desarrollo.

####Edit - Pagina de edicion

* Lo sentimos, seccion en desarrollo.

####Update - Funcion de actualizacion

* Lo sentimos, seccion en desarrollo.

####Destroy - Funcion de borrado

Este metodo tiene la funcion de eliminar el registro de la BD, pero para efectuarlo tenemos dos opciones, la **primer** forma: crear una variable ```$pastel``` y despues usar el metodo ```delete()``` de Eloquent. o bien la **segunda**: directamente del modelo usar el metodo de Eloquent ```destroy($id)```, que se encarga de directamente buscar y eliminar el registro, finalmente vamos a redirigir al index, el metodo al final quedara de la siguiente forma:

```
	public function destroy($id)
    {
        $pastel = Pastel::find($id);
        $pastel->delete();

        Pastel::destroy($id);
        return redirect()->route('pasteles.index');

    }
```

#Vistas del CRUD

Estas son la ultima parte que vamos a crear, primero debemos preparar los directorios y los archivos que usaremos.

La estructura de la carpeta quedaria de la siguiente forma:

```
resources/
	views/
		pasteles/
			partials/
				fields.blade.php
				table.blade.php
			index.blade.php
			create.blade.php
			edit.blade.php
```

Usaremos el **template** por defecto de Laravel llamado ```app.blade.php``` que fuimos modificando durante el curso por lo cual solo deberemos crear los archivos restantes.

###**Contenido incompleto, lo sentimos proximamente se completara la seccion**
