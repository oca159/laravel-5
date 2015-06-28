# Preparando nuestro entorno de trabajo.

Laravel necesita un servidor web. No importa cuál sea pero la mayoría de la comunidad usa Apache o Nginx y hacer lo mismo te pondrá las cosas más fáciles a la hora de buscar ayuda si la necesitas.

## Instalación de XAMPP (Windows)
XAMPP es un programa que nos ofrece una distribución de Apache, MySQL, PHP y Perl muy simple de instalar, administrar y utilizar.
Podemos descargarlo  [aquí](https://www.apachefriends.org/es/download.html).

## Instalación de LAMP (Linux)
LAMP es el conjunto de aplicaciones Apache, MySQL, PHP o Python en entornos Linux que nos facilitan el desarrollo de sistemas.

En Ubuntu o derivadas podemos instalarlo con los siguientes comandos:

```shell
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install lamp-server^
sudo apt-get install php5-mcrypt
sudo php5enmod mcrypt
```
----
Despues de tener instalado nuestro Servidor web, es necesario instalar composer el cuál es un gestor de dependencias php muy útil y del cuál se hablará más tarde. 
## Instalación de composer (Windows)
La forma más sencilla de instalar Composer en tu ordenador Windows consiste en descargar y ejecutar el archivo [Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe), que instala la versión más reciente de Composer y actualiza el PATH de tu ordenador para que puedas ejecutar Composer simplemente escribiendo el comando composer.

## Instalación de composer (Linux)
En ubuntu bastará con ejecutar los siguientes comandos en la terminal.
```
sudo apt-get install curl
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
sudo echo 'PATH=$PATH:~/.composer/vendor/bin' >> ~/.profile
```
---
# Instalación de Laravel 

Existen diferentes formas de instalar laravel en nuestra computadora.
* Podemos clonar el repositorio
 [Laravel](https://github.com/laravel/laravel) de github. 
* Usando el instalador:
```php
  composer global require "laravel/installer=~1.1"
  laravel new Proyecto
```
* Usando composer:
```php
composer create-project laravel/laravel --prefer-dist Proyecto
```
Una vez instalado laravel es recomendable situarse en la raíz del proyecto y ejecutar: 
```php
composer update
php artisan key:generate
php artisan app:name Curso
```

