#PSR-4 y namespaces

####¿Qué es PSR-4?
Es una especificación para la auto carga de clases desde la ruta de los archivos. Describe dónde se encuentran ubicados los archivos que serán autocargados.
PSR-4 hace uso de namespaces para distinguir una clase de otra, esto es de gran ayuda cuando ocupamos librerías de terceros porque en muchas ocaciones existirán clases con el mismo nombre que las nuestras y podrían sobreescribirse o usar una que no queremos.

PSR-4 fue creada por el grupo de interoperabilidad de PHP, ellos han trabajado en la creación de especificaciones de desarrollo para este lenguaje para que estandarizemos diferentes procesos, como es en este caso el como nombrar las clases de nuestro proyecto y hacer uso de ellas.

Usar especificaciones PSR-4 no es obligatorio y su uso puede ser completo o parcial, aunque es recomendable no omitirlo porque a Composer le permite cargar nuestras clases automaticamente.

####¿Qué es un autoloader?
Aparecieron desde la versión de PHP5 y nos permite encontrar clases para PHP cuando llamamos las funciones new() o class_exists(). De esta forma no tenemos que seguir haciendo uso de require() o include().

PSR-4 nos permite definir namespaces de acuerdo a la ruta de los archivos de las clases, es decir, si tenemos una clase "Pdf" en el directorio Clases/Templates/ , ese será su namespace.
Podemos hacer un simil con el import de java.

El namespace de Clases/Templates quedaría de la siguiente forma:
Clases\Templates\Pdf.php

Para usar PSR-4 en composer podemos definir el namespace de nuestra aplicación y el directorio dónde serán alojadas las clases, por ejemplo:
```
{
	"autoload":{
	    "psr-4":{
			"Taller\\": "app/"
		}
	}
}
```

Para usar los namespaces dentro de nuestros archivos php basta con referenciarlos de la siguiente forma:

    use Taller\Clase;

####¿Qué es classmap?
Es un autoloader que nos permite registrar nuestras clases para poder ocuparlas sin necesidad de un namespace, la desventaja respecto a PSR-4 es la colisión de clases con mismo nombre, la principal ventaja es la rápidez de autocarga de clases.
Otro inconveniente de usar classmap es que debemos ejecutar constantente el comando **"composer dump-autoload"** por cada clase nueva en el directorio que indiquemos o tengamos registrado en el archivo **"composer.json"**.

Ejemplo:
```
{
	"classmap": [
		"database"
	],
}
```