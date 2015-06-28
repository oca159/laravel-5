#Modelos y uso de  Eloquent

###Eloquent

En Laravel podemos hacer uso de un [**ORM**](https://es.wikipedia.org/wiki/Mapeo_objeto-relacional) llamado Eloquent, un [**ORM**](https://es.wikipedia.org/wiki/Mapeo_objeto-relacional) es un ***Mapeo Objeto-Relacional*** por sus siglas en ingles (Object-Relational mapping), que es una forma de mapear los datos que se encuentran en la base de datos almacenados en un lenguaje de script SQL a objetos de PHP y viceversa, esto surge con la idea de tener un codigo portable con el que no tengamos la necesidad de usar lenguaje SQL dentro de nuetras clases de PHP.

Eloquent hace uso de los **Modelos** para recibir o enviar la información a la base de datos, para esto analizaremos el modelo que viene por defecto en Laravel, este es el modelo **User** que se ubica en la carpeta ```app/```, los modelos hacen uso de PSR-4 y namespaces, un modelo nos ayuda a definir que tabla, atributos se pueden llenar y que otros se deben mantener ocultos.

Los modelos usan convenciones para que a Laravel se le facilite el trabajo y nos ahorre tanto lineas de codigo como tiempo para relacionar mas modelos, las cuales son:

* El nombre de los modelos se escribe en singular, en contraste con las tablas de la BD que se escriben en plural.

* Usan notacion UpperCamelCase para sus nombres.

Estas convenciones nos ayudan a detectar automaticamente las tablas, por ejemplo: el modelo **User** se encuentra en **singular** y con notacion **UpperCamelCase** y para Laravel poder definir que tabla es la que esta ligada a este modelo le es suficiente con realizar la conversion a notacion **underscore** y **plural**, dando como resultado la tabla: **users**.

Y esto aplica para cuando queremos crear nuestros modelos, si tenemos una tabla en la base de datos con la que queremos trabajar que se llama **user_profiles**, vemos que se encuentra con las convenciones para tablas de bases de datos (plural y underscore), entonces el modelo para esta tabla cambiando las convenciones seria: **UserProfile** (singular y UpperCamelCase).

Retomando el ejemplo que vimos en el [Capítulo 5](capítulos/chapther5.md) sobre la migracion de pasteles, crearemos ahora un modelo para poder trabajar con esa tabla, el cual recibira el nombre de **Pastel** y el comando para poder crear nuestro modelos es:

```shell
php artisan make:model Pastel
```
Con esto se generara el archivo en donde ya se encuentra el modelo **User** en la carpeta ```app/``` y dentro de el vamos a definir la tabla que se va a usar con esta linea:

```php
protected $table = 'pasteles';
```
#####¿Pero no se suponia que Laravel identificaba automaticamente que tabla usar?

Si lo hace pero si cambiamos las convenciones del modelo **Pastel** el resultado seria **pastels** y nuestra tabla se llama **pasteles**, esto es un problema para nosotros por el hecho del uso del lenguaje español porque la conversion de singular a plural no es la misma que la forma en que se hace en ingles, debido a esto nos vemos forzados a definir el nombre de la tabla.

Bien una vez creado nuestro modelo pasaremos a crear una ruta de tipo **get** en nuestro archivo ***routes.php*** que se vio en el [Capítulo 8](capítulos/chapther9.md) sobre enrutamiento básico, que quedaria de la siguiente forma:

```php
Route::get('pruebasPastel', function(){

});
```

Dentro de esta ruta de prueba vamos a usar nuestro modelo, pero como estamos usando la especificacion PSR-4 debemos incluir el namespace del modelo, que seria igual a esto:

```php
use Curso\Pastel;
```

Con esto estamos diciendo que incluya la clase **Pastel** que es nuestro modelo, y con esto podemos ya hacer consultas a nuestra BD y mapear a objetos PHP. En la [documentacion oficial](http://laravel.com/docs/5.0/eloquent) de Laravel podemos ver todas las opciones que nos permite **Eloquent**, unas de las instrucciones basicas de este son ***get()*** que nos regresa todos los registros de la BD y ***first()*** que nos regresa el primer registro de una seleccion.

A su vez podemos unir esto a mas filtros de seleccion SQL, como por ejemplo seleccionar el primer pastel de vainilla, la sintaxis de Eloquent seria la siguiente:

```php
$pastel = Pastel::where('sabor','vainilla')->first();
```

Esto nos va a dar el primer pastel sabor vainilla, pero si quisieramos todos los pasteles de vainilla cambiariamos el metodo ```first()``` por el metodo ```get()``` para obtener todos.

Y si queremos ver el resultado de esto y que de verdad estamos haciendo lo correcto podemos usar la funcion ```dd()``` para mostrar en pantalla el valor de una variable, con esto entonces nuestra ruta le agregariamos lo siguiente:

```php
Route::get('pruebasPastel', function(){
	$pasteles = Pastel::where('sabor','vainilla')->get();
	dd($pasteles);
});
```

Y en el navegador deberiamos ver algo como esto:

![](images/collection.png)

Esto es la función ```dd($pasteles)``` mostrando el contenido de la variable ```$pasteles```. Ahora bien si tuvieramos la necesidad de realizar siempre un mismo filtro, Eloquent nos provee de una herramienta llamada **scopes** que lo que realizan son consultas en especifico encapsulandolas dentro de funciones en el modelo, por ejemplo si quisieramos que el modelo **Pastel** tuviera una funcion que me diera todos los pasteles de vainilla, otra de chocolate y otra función mas para cheesecake, entonces podria crear un scope para cada una.

Con el ejemplo de la ruta pruebasPastel para el sabor vainilla:

```php
	public function scopeVainilla($query){
    	return $query->where('sabor','vainilla');
    }
```

Los scopes en la función se debe iniciar el nombre de la función con la palabra **scope** y seguido en notacion camelCase el nombre con el cual se va a llamar el scope. Y su equivalente dentro de la ruta seria la siguiente:

```php
	Route::get('pruebasPastel', function(){
		$pasteles = Pastel::vainilla()->get();
		dd($pasteles);
	});
```

También podemos crear scopes dinamicos de la siguiente forma:

```php
	public function scopeSabor($query, $sabor){
    	return $query->where('sabor',$sabor);
    }
```
Esto nos daria una función generica para obtener los pasteles de cierto sabor y su implementacion seria asi:

```php
	Route::get('pruebasPastel', function(){
		$pasteles = Pastel::sabor('vainilla')->get();
		dd($pasteles);
	});
```

Además con Eloquent tambien podemos insertar, actualizar o eliminar registros, por ejemplo:

Para insertar la sintaxis seria la siguiente:

```php
$pastel = new Pastel;

$pastel->nombre = 'Pastel Richos Style';
$pastel->sabor  = 'chessecake';

$pastel->save();

```

Para actualizar seria la siguiente:

```php
$pastel = Pastel::find(51);

$pastel->sabor = 'chocolate';

$pastel->save();
```

Para eliminar seria la siguiente:

```php
$pastel = Pastel::find(51);

$pastel->delete();
```

o bien podriamos destruir el registro directamente con el modelo si tenemos su ID:

```php
Pastel::destroy(51);
```
