# JSON
JSON es un acrónimo de JavaScript Object Notation, un formato ligero originalmente concebido para el intercambio de datos en Internet. JSON nos permite representar objetos, arrays, cadenas, booleanos y números.

La ventaja de usar JSON para la transferencia de información es que puede ser parseada por varios lenguajes y es un formato estandarizado, es decir que cualquier lenguaje puede intercambiar datos con otro mediante JSON.

Por defecto, JSON se guarda sin espacios entre sus valores lo cual lo puede hacer un poco más difícil de leer.
Esto se hace normalmente para ahorrar ancho de banda al transferir los datos, sin los espacios en blanco adicionales, la cadena JSON será mucho más corta y por tanto habrá menos bytes que transferir.

Sin embargo, JSON no se inmuta con los espacios en blanco o saltos de linea entre las claves y valores, así que podemos hacer uso de ellos para hacerlo un poco más legible.
JSON es un formato de transferencia de dato y no un lenguaje.

Debemos tener siempre en cuenta que en el formato JSON las cadenas siempre van en comillas dobles, además, los elementos clave y valor debes estar separadas con dos puntos (:), y las parejas clave-valor por una coma (,).
```json
{ 
    "Frutas":[
        {
            "Nombre": "Manzana",
            "Cantidad": 20,
            "Precio": 10.50,
            "Podrida": false 
        },
        {
            "Nombre": "Pera",
            "Cantidad": 100,
            "Precio": 1.50,
            "Podrida": true 
        }
    ]
    
}
```

### Los tipos de valores aceptados por JSON
Los tipos de valores que podemos encontrar en JSON son los siguientes:

* Un número (entero o flotante)
* Un string (entre comillas dobles)
* Un booleano (true o false)
* Un array (entre corchetes [] )
* Un objeto (entre llaves {})
* Null

### ¿Por qué aprender JSON?
JSON es utilizado ampliamente en:
* El archivo composer.json de proyectos PHP
* Intercambio de información
* Representación de una base de datos
* [AJAX](https://es.wikipedia.org/wiki/AJAX)
* [Web Services](https://es.wikipedia.org/wiki/Servicio_web)


### Validación de JSON
JSON es un un formato para el intercambio de información muy rígido y estricto, si tenemos un error de sintaxis, obtendremos un error y no podremos parsear el JSON.
Para solucionar este tipo de problemas, existen en internet un gran número de herramientas que nos ayudan a escanear y encontrar posibles errores en la formación de nuestro JSON.

Podemos ocupar [JSONLint](http://jsonlint.com/) que es una muy buena opción, bastará con copiar y pegar nuestro JSON en el área de texto y a continuación dar click en el botón "Validate". 

JSONLint nos informará si es correcto el formato o en caso contrario nos mostrará los errores sintácticos de nuestro JSON.




