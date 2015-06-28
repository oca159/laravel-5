#Controladores

En lugar de definir la totalidad la logica de las peticiones en el archivo routes.php, es posible que desee organizar este comportamiento usando clases tipo Controller. Los **Controladores** puede agrupar las peticiones HTTP relacionada con la manipulación lógica en una clase. Los **Controladores** normalmente se almacenan en el directorio de aplicación ```app/Http/Controllers/```.

Un controller usualmente trabaja con las peticiones:

* **GET.**
* **POST.**
* **PUT.**
* **DELETE.**
* **PATCH.**

Asociando los metodos de la siguiente forma:

* **GET:** index, create, show, edit.
* **POST:** store.
* **PUT:** update.
* **DELETE:** destroy.
* **PATCH:** update.



Los controladores nos ayudan a agrupar estas peticiones en una clase que se liga a las rutas, en el archivo ```app/Http/routes.php```, para esto usamos un tipo de ruta llamana resource:

```
Route::resource('pasteles', 'PastelesController');
```

Esta ruta nos creara un grupo de rutas de recursos con las peticiones que estas mencionadas arriba: **index, create, show, edit, store, update, destroy**. Estas son las operaciones mas usadas en una clase y para no tener que crear una ruta para cada metodo es que Laravel agrupa todo esto con una ruta de tipo resource que se liga a un controlador.

Estos metodos significan:

* index: Es el metodo inicial de las rutas resource, usualmente lo usamos para mostrar una vista como pagina principal que puede contener un catalogo o resumen de la informacion del modelo al cual pertenece o bien no mostrar informacion y solo tener la funcion de pagina de inicio.

* create: Este metodo lo podemos usar para direccionar el sistema a la vista donde se van a recolectar los datos(probablemente con un formulario) para despues almacenarlos en un registro nuevo, usualmente redirige al index.

* show: Aqui podemos hacer unna consulta de un elemento de la base de datos o de todos los elementos o registros por medio del modelo para realizar una descripcion.

* edit: Este metodo es similar al de **create** porque lo podemos usar para mostrar una vista que recolecta los datos pero a diferencia de create es con el fin de actualizar un registro.

* store: Aqui es donde se actualiza un registro en especifico que proviene del metodo create y normalmente redirige al index.

* update: Al igual que el store, solo que en vez de provenir de create proviene de **edit** y en vez de crear un nuevo registro, busca un existente y lo modifica, tambien suele redirigir al index.

* destroy: En este metodo usualmente se destruye o elimina un registro y la peticion puede provenir de donde sea siempre y cuando sea llamado con el metodo **DELETE**, despues puede redirigir al index o a otro sitio dependiendo si logro eliminar o no.

Ahora esto no quiere decir que un controlador necesariamente debe ejecutar estas peticiones obligatoriamente, podemos omitirlas o incluso agregar mas.

Para crear controladores en Laravel usamos artisan con el siguiente comando:

```
php artisan make:controller NameController
```

El comando anterior creara un controlador en la carpeta ```app/Http/Controllers/``` que por defecto va a tener todos estos metodos dentro de si, entonces agregaremos la ruta de tipo resourse anterior al archivo de rutas y correremos el siguiente comando en la consola:

```
php artisan make:controller PastelesController
```

Con esto vamos a poder trabajar para cada metodo del controlador una ruta y las funciones internas son las que se van a ejecutar, el archivo creado se vera de la siguiente manera:

![](images/PastelesController.png)

Con todos los metodos podemos examinar por linea de comandos todas las rutas que nuestro proyecto tiene registradas:

```
php artisan route:list
```

Este comando nos va a mostrar en la consola un resultado similar a esto:

![](images/route_list.png)

aqui podemos ver el nombre de nuestras rutas, de que tipo son, si es que reciben parametros y como se llaman, esta informacion es muy util para poder asociar los metodos del controlador con las rutas y tambien como es que las vamos a usar en el navegador.

Por ejemplo la ruta ```pateles/{pasteles}```  de tipo **GET** con el nombre ***pasteles.index***, se asocia a la funcion **index()** del controlador PastelesController y por consecuente lo que hagamos en esa funcion lo podremos ver en el navegador.

Los controladores son un tema complicado y extenso asi como el enrutamiento aunque en el curso solo vimos [enrutamiento basico](chapter8.md), por lo cual dejamos los links de la documentacion oficial de [Controladores](http://laravel.com/docs/5.1/controllers) y de [Enrutamiento](http://laravel.com/docs/5.1/routing) en la version 5.1 de Laravel.
