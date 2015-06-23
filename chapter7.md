# Estructura de un proyecto en Laravel
Todos los proyectos nuevos en Laravel 5.1 tienen la siguiente estructura de directorios:
* app/
* bootstrap/
* config/
* database/
* public/
* resources/
* storage/
* tests/
* vendor/
* .env
* .env.example
* .gitattributes
* .gitignore
* artisan
* composer.json
* composer.lock
* gulpfile.js
* package.json
* phpspec.yml
* phpunit.xml
* readme.md
* server.php

A continuación describiremos los directorios y archivos más importantes que nos ayuden a entender más el funcionamiento del framework.

### El directorio app

App es usado para ofrecer un hogar por defecto a todo el código personal de tu proyecto. Eso incluye clases que puedan ofrecer funcionalidad a la aplicación, archivos de configuración y más. Es considerado el directorio más importante de nuestro proyecto ya que es en el que más trabajaremos.

El directorio app tiene a su vez otros subdirectorios importantes pero uno de los más utilizados es el directorio **Http** en el cuál ubicaremos nuestros `Controllers`, `Middlewares` y `Requests`en sus carpetas correspondientes, además dentro del subdirectorio **Http** encontremos también el archivo `routes.php` el cuál es el archivo en el que escribiremos las rutas de la aplicación.

A nivel de la raíz del directorio app encontraremos el modelo `User.php`, los modelos comunmente se ubicarán a nivel de la raíz de la carpeta app aunque igual es posible estructurarlos de la forma que queramos en una carpeta llamada `Models`por ejemplo.
![](app.png)
