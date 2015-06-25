# Conexión con bases de datos
Laravel tiene soporte para los motores de bases de datos más populares como:
* MySQL
* Postgresql
* SQLite3
* SQL Server
    

Veremos como utilizar MySQL con laravel.

Dentro del archivo `database.php` en el directorio `config` configuramos el driver de la conexión, por defecto vendrá con mysql, si queremos cambiarlo por otro motor de base de datos tendremos que cambiar el valor `mysql` por sqlite, pgsql, sqlsrv.

```
'default' => env('DB_CONNECTION', 'mysql')
``` 

Tendremos que configurar el archivo `.env` ubicado en la raíz del proyecto.
```
DB_HOST=localhost
DB_DATABASE=curso
DB_USERNAME=root
DB_PASSWORD=12345
``` 

Una vez que tengamos todo configurado, nos dirigimos a la terminal y ejecutamos el comando `php artisan migrate` para crear las migraciones, si todo ha salido bien tendremos que ver las tablas: 
* migrations
* password_resets
* users

Si eres una persona curiosa habrás notado que el nombre de las tablas en Laravel siempre son escritas en plural, esto no es por puro capricho, es parte de una convención: **** Convención de la configuración ****, dicha convención le permite a Laravel hacer magía por nosotros, nos evita realizar configuración y pasos extras de la asociación de Modelos con tablas entre otras cosas.











