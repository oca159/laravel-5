#Migraciones 

Cuando creamos nuestras bases de datos solemos crear diagramas que nos facilitan la abstracción de como se va a almacenar nuestra información, pero la forma de llevarlo a la realidad en algun gestor de bases de datos, como por ejemplo: **MySQL**, **SQLite**, **PostgreSQL**, **SQL Server**, etc., lo más comun es meternos al lenguaje de script encargado de implementar nuestra idea de la BD y ejecutar dicho script, o incluso ocupar programas más avanzados que nos sirven como interfaz para crearlas de una forma más gráfica y sin la necesidad de profundizar demasiado en el lenguaje, como **Workbench** o **Navicat**.

En Laravel se lleva a otro contexto esta situación, puesto que visto de la forma tradicional si se requieren cambios en la base de datos tenemos que meternos ya sea a otro programa para cambiar el diagrama de la base o a un archivo SQL con una sintaxis usualmente complicada o difícil de leer y ejecutar los cambios para reflejarlos en el proyecto, sin embargo, debido a que con esto no contamos son un control de los cambios (control de versiones) sobre la base de datos, si necesitamos consultar un cambio anterior o de repente la solución previa o inicial era la que se necesita al momento debemos re-escribir todo otra vez, cosa que con la **migraciones** se soluciona instantaneamente.

Las migraciones son archivos que se encuentran el la ruta `database/migrations/` de nuestro proyecto Laravel, por defecto en la instalación de Laravel 5 se encuentran dos migraciones ya creadas, ***create_users_table*** y ***create_password_resets_table***. 

Para crear nuestras migraciones en Laravel se usa el siguiente comando:

```
php artisan make:migration nombre_migracion
```
que nos crea el archivo limpio para escribir nuestra migración, o bien el comando:
```
php artisan make:migration nombre_migracion --create=nombre_tabla
```

que nos agrega una plantilla de trabajo básica para empezar a trabajar.

Como ejemplo del curso se tomara este comando:

```
php artisan make:migration crear_tabla_pasteles --create=pasteles
```
el cual nos dará este resultado:

```
Created Migration: 2015_06_23_054801_crear_tabla_pasteles
```
Y nos creará además el siguiente archivo:

![](Screenshot - 230615 - 00:48:22.png)

Ahora bien se puede observar que el archivo como tal no se llama simplemente ***crear_tabla_pasteles*** sino ***2015_06_23_054801_crear_tabla_pasteles***, esto pasa porque Laravel al crear una migración agrega como préfijo la fecha y hora en la que fué creada la migración para poder ordenar qué migración va antes que otra, por lo cual si tu ejecutas este comando, obviamente el nombre de tu archivo será diferente pues la fecha y hora no pueden ser las mismas que la mia al crear este ejemplo. Además las migraciones que vienen por defecto en Laravel también se encuentran con este formato por lo cual podemos observar que para todos estos dos archivos si tienen el mismo nombre exactamente. 

Dentro de la estructura del archivo podemos ver dos funciones, una llamada **up()** y otra llamada **down()**, la primer función es en donde vamos a especificar la estructura de nuestra tabla, inicialmente y gracias al comando se encuentran ya algunas cosas esscritas como lo son la clase **Schema** en la cual se llama al método **create**, el cual nos permite crear la tabla en nuestra base de datos, esta recibe dos parámetros, el primero es el nombre que va a recibir la tabla que si no mal recuerdan es el que se le dio en el comando y por lo cual ya se encuentra en su lugar, y el segundo parámetro es una función closure o función anónima que lo que hace es definir las columnas de nuestra tabla, a su vez esta función anónima recibe como parámetro un objeto de tipo **Blueprint** que se agregó dentro del namespace con la palabra **use** en la cabecera del archivo, el objeto `$table` es con el que vamos a trabajar para definir los campos, como se ve en la imagen anterior esto se logra escribiendo `$table->tipo_dato('nombre');`, y esto puede variar dependiendo el tipo de dato que se use y para ello podemos revisar la documentación oficial de Laravel [aquí](http://laravel.com/docs/5.0/schema#adding-columns) para poder ver todos los tipos de campos con los que contamos.

En el ejemplo observamos que ya tenemos el campo `'id'` de tipo `increments` que es equivalente a un campo en SQL así:

```sql
create table pasteles (id int auto_increment);
```
Y un campo de tipo `timestamps` sin nombre, el efecto que tendrá será agregar dos columnas muy útiles que son `created_at` y `updated_at` que son campos que se usan para (como su nombre lo dice) guardar el registro de cuando fue creado y cuando fue actualizado el registro, detalles muy importantes cuando queremos obtener informes con base en el tiempo de la información de nuestra tabla, por ejemplo si quisieramos saber cuales son los pasteles que se dieron de alta en el catálogo en el mes de abril podriamos crear un filtro para obtener solo los pasteles de ese mes usando el campo generado `created_at`.

Ahora bien si la función **up** crea nuestra tabla en la base de datos, la función **down** logicamente hace lo opuesto, y eso es eliminar la tabla de la base de datos, por eso dentro de esta función podemos observar que de la misma clase `Schema` se llama al método `drop` que significa dejar caer o dar de baja.

Si bien cada función realiza una tarea en especifico, ¿Cuando es que se usan? o ¿Como se mandan a llamar?. Bueno para esto iremos nuevamente a nuestra linea de comandos.

Para correr o iniciar nuestras migraciones usamos el comando:
```
php artisan migrate
```
Con esto si es la primera vez que se ejecuta este comando se creará en nuestra base de datos la tabla **migrations** que es la encargada de llevar el control de que migraciones ya han sido ejecutadas, con el fin de no correr el mismo archivo más de una vez si el comando se usa nuevamente.

Entonces si creamos nuestra migración ***crear_tabla_pasteles*** y usamos el comando ```php artisan migrate``` como resultado en nuestra base de datos se agregará la tabla **pasteles** y en la tabla **migrations** se añadirá el registro de la migración recien ejecutada.

Pero, ¿si quisiera eliminar la tabla con la función down de la migración ***crear_tabla_pasteles***?

Esto se puede resolver de dos formas básicamente:

1. Con el comando ```php artisan migrate:rollback``` que lo que hará es deshacer la última migración ejecutada y registrada en la base de datos.

2. Con el comando ```php artisan migrate:reset``` que lo que hará es deshacer todas las migraciones de la base de datos.


* **Nota**: Un comando extra que nos permite actualizar las migraciones es el comando ```php artisan migrate:refresh```, el cual es equivalente a usar ```php artisan migrate:reset``` y después ```php artisan migrate```.

En el dado caso que necesitaramos agregar más campos a la tabla pasteles, podríamos simplemente ir a la migración ***crear_tabla_pasteles*** y en la función **up** poner la nueva columna, pero con esto perderiamos la primer versión de la tabla, entonces para poder ejeplificar como se agregan columnas con las migraciones crearemos una nueva que se llame ***agregar_campos_tabla_pasteles*** con los comandos que ya hemos visto:

1. Primero ejecutamos el comando: ```php artisan make:migration agregar_campos_tabla_pasteles```, para crear la migración simple sin la plantilla.

2. Dentro de la función **up** agregamos los campos que necesitamos, en este caso solo agregaremos el nombre y el sabor.

3. Después como la función **down** hace lo opuesto que la función **up**, dentro de esta eliminaremos los campos recien agregados.

Ahora el archivo resultante quedaría así:

![](Screenshot - 230615 - 01:56:19.png)

Para poder agregar más columnas a las tablas desde Laravel en vez de llamar al método ```create``` llamamos al método ```table``` de la clase ```Schema``` pasandole como primer parámetro a que tabla se va a agregar los campos y como segundo parámetro la función anónima donde definimos que columnas se agregaran.

Y en la función **down** para eliminar columnas que vendría siendo lo opuesto de agregarlas, se llama al método ```table``` y dentro de la función anónima del objeto ```$table``` se usa el método ```dropColumn()``` que recibe como parámetro ya sea el nombre de una sola columna o un arreglo con todas las columnas que se desean eliminar.

Y listo!, con esto podemos tener una idea inicial de como usar las migraciones, lo que para este ejemplo podría continuar sería agregar más columnas a la tabla pasteles y probar los comandos necesarios para poder deshacer los cambios de la primera vez que se corrio la migración con una nueva versión, ya sea sobre el mismo archivo o sobre otro nuevo.

###Beneficios

* Tenemos un mayor control de las versiones de la base de datos.

* Podemos con un simple comando ver reflejados los cambios de nuestra base de datos.

* El lenguaje en el cual se trabaja sigue siendo PHP, por lo cual no se diferencia tanto de lo que ya nos acostumbraremos con Laravel.

* La ultima versión de nuestra base siempre estará actualizada para todos los miembros del equipo de trabajo si usamos un control de versiones como **GIT**.

* Provee de portabilidad para diferentes gestores, usando el mismo código.

#Seeders

Los Seeders por otra parte son archivos que nos van a permitir poblar nuestra base de datos para no tener que perder el tiempo escribiendo de forma manual todos los datos, un ejemplo sería imaginar llenar 15 tablas con 100 registros cada una y pensar en que entre cada tabla deben existir registros que se relacionan entre sí, eso suena de verdad horrible y tedioso, por lo cual Laravel nos salva con estos archivos Seeders.

Un Seeder se ubica en la carpeta ```database/seeds/``` de nuestro proyecto de Laravel y para poder crear un nuevo Seeder se usa el comando:

```
php artisan make:seeder nombre_seeder
```
Esto nos creará un archivo en la carpeta ```database/seeds/``` que tendrá el nombre que le demos en el comando, por ejemplo crearemos uno retomando el ejemplo anterior de las migraciones, se llamará PastelesSeeder, por lo cual el comando quedaria de la siguiente forma:

```
php artisan make:seeder PastelesSeeder
```

Con esto ya tenemos el archivo pero no es todo lo que necesitamos para poder trabajar con datos autogenerados, para ello usaremos un componente llamado **Faker** el cual se encargará de generar estos datos, por defecto el boilerplate del proyecto de Laravel 5.1 que estamos trabajando viene ya con **Faker** dentro del composer.json por lo cual ya debe estar instalado dentro de nuestro proyecto, ahora bien si estamos trabajando con una instalación Laravel 5.0 sin el componente **Faker** basta con ir al archivo composer.json y agregar en el **"required-dev"** las dependencias y para tener una idea más clara podemos ir a la página de [Packagist](https://packagist.org/packages/fzaninotto/faker) donde se encuetra **Faker** o a su [Repositorio](https://github.com/fzaninotto/Faker) en Github y ahí nos muestra que es lo que se debe agregar.

Al final solo se ocupa el comando ```composer update``` para actualizar las dependencias y descargar **Faker** al proyecto.

Una vez ya teniendo **Faker** iremos a nuestro archivo ***PastelesSeeder*** y dentro podremos observer que se encuentra una funnción llamada ```run()``` que es donde nosotros vamos a usar **Faker** para poblar, ahora bien antes de todo debemos agregar la clase de **Faker** a nuestro Seeder, para esto agregamos al inicio del archivo la linea:
```
use Faker\Factory as Faker;
```
Quedando el archivo de la siguiente forma:

![](Screenshot - 230615 - 02:52:52.png)

Después crearemos una variable llamada ```$faker``` que nos servira para poblar la base de datos, ahora bien usando la clase DB, si bien dentro del ejemplo queremos crear 50 pastelesvamos a crear un for para que ejecute nuestro código de inserción 50 veces y el componente de **Faker** en cada pasada cambiará los valores del registro que se va a agregar, quedando de esta forma:

```php
$faker = Faker::create();
for ($i=0; $i < 50; $i++) { 
    \DB::table('pasteles')->insert(array(
       	'nombre' => $faker->firstNameFemale,
       	'sabor'  => $faker->randomElement(['chocolate','vainilla','cheesecake']),
       	'created_at' => date('Y-m-d H:m:s'),
       	'updated_at' => date('Y-m-d H:m:s')
    ));
}
```

Creamos nuestro objeto Faker, el cual puede generar información falsa para nuestra base de datos y ahora usamos la clase ```DB``` el método ```table``` para llamar la tabla donde se va a insertar la información y se le concatena el método ```insert()``` el cual recibe por parametro un arreglo ```clave => valor``` con los campos de la tabla.

Faker tiene muchas variedades de datos, los cuales podemos consultar en su [Repositorio](https://github.com/fzaninotto/Faker) de Github así como su uso básico.

En este ejemplo usamos una propiedad que se llama **firstNameFemale** para darle nombre al pastel y la propiedad **randomElement** que de un arreglo que se le da asigna un elemento de ese arreglo aleatoriamente.

Y ahora lo que sigue es abrir un archivo llamado ```DatabaseSeeder.php```, en este archivo se mandan a llamar todos los seeders en el orden que los necesitemos, en este archivo se agregará la linea:
```
$this->call('PastelesSeeder');
```

que en si mandará a llamar nuestro seeder recien creado y para ejecutar este archivo se usa el comando:

```
php artisan db:seed
```

Y con esto queda poblada la tabla ***pasteles*** y lo puedes verificar en tu gestor de base de datos.

Cuando trabajamos con Migraciones y Seeder por primera vez puede parecer un poco más complicado que a lo que estamos acostumbrados pero las ventajas que nos da superan por mucho a la forma convencional, adempas de ser una forma más profesional de trabajar.

Unos comandos extras que nos pueden ser utiles son:

```
php artisan migrate --seed
```

El comando anterior lo que hace es realizar una combinación entre los comandos ```php artisan migrate``` y ```php artisan db:seed```.

```
php artisan migrate:refresh --seed
```

El comando anterior lo que hace es realizar una combinación entre los comandos ```php artisan migrate:refresh``` y ```php artisan db:seed```.