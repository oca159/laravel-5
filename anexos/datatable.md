# DataTable
Los DataTables son un plug-in para la librería JQuery de Javascript. Nos permiten un mayor control sobre un elemento `<table>` de HTML. Algunas de sus caracteristicas principales son:

* Paginación automática
* Búsqueda instantánea
* Ordenamiento multicolumna
* Soporta una gran cantidad de datos fuente.
  * [DOM](https://es.wikipedia.org/wiki/Document_Object_Model) (Convertir un elemento HTML `<table>` en un DataTable).
  * Javascript (Un arreglo de arreglos en Javascript puede ser el dataset de un DataTable).
  * AJAX (Un DataTable puede leer los datos de una tabla de un json obtenido vía AJAX, el json puede ser servido mediante un Web Service o mediante un archivo.txt que lo contenga).
  * Procesamiento del lado del servidor (Un DataTable puede ser creado mediante la obtención de datos del procesamiento de un script en el lado del servidor, este script comúnmente tendrá interacción con la base de datos).
* Habilitar temas CSS para el DataTable fácilmente.
  * [Crear un tema](https://www.datatables.net/manual/styling/theme-creator)
  * [Tema JQuery UI](https://www.datatables.net/manual/styling/jqueryui)
  * [Tema Bootstrap](https://www.datatables.net/manual/styling/bootstrap)
  * [Tema Foundation](https://www.datatables.net/manual/styling/foundation)
* Amplia variedad de extensiones.
* Altamente internacionalizable.
* Es open source (Cuenta con una licencia MIT).

# ¿Cómo usar DataTables?
Para ocupar datables tenemos dos formas:
* Descargar el código fuente de los archivos js y css de datatables en el siguiente [link](https://www.datatables.net/download/index).  
* Ocupar un [CDN](https://es.wikipedia.org/wiki/Red_de_entrega_de_contenidos)
  * **CSS** `//cdn.datatables.net/1.10.7/css/jquery.dataTables.min.css`
  * **JS**    `//cdn.datatables.net/1.10.7/js/jquery.dataTables.min.js`


#### Usar DataTable descargando el código fuente
Antes que nada debemos tener descargado jquery, el cuál podemos descargar de [aquí](https://jquery.com/download/).
Una vez que hemos descargado los archivos css y js de DataTables de este [link](https://www.datatables.net/download/index) debemos extraer el contenido y ubicar los archivos en la carpeta css y js correspondiente dentro del directorio **public** de Laravel.

Ejemplo:
```
public/
  assets/
    css/
      datatable-bootstrap.css
    js/
      datatable-bootstrap.js
      jquery.dataTables.js
      jquery.js
```
Lo siguiente será hacer una referencia de los script y archivos css dentro de nuestro documento HTML.

Los script los ubicaremos dentro de la etiqueta `<body>` pero hasta el final de la misma.

```html
<body>
<!-- Contenido -->

<!-- Scripts -->
    {!! Html::script('assets/js/jquery.js') !!}
    {!! Html::script('assets/js/jquery.dataTables.js') !!}
    {!! Html::script('assets/js/bootstrap.min.js') !!}
    {!! Html::script('assets/js/datatable-bootstrap.js') !!}
    <script>
        $(document).ready(function(){
            $('#MyTable').dataTable();
        });
    </script>
</body>
```

En el ejemplo de arriba estamos indicando que la tabla con el id `#MyTable` será un elemento de tipo dataTable. La obtención de datos la hacemos mediante el [DOM](https://es.wikipedia.org/wiki/Document_Object_Model).

Por otro lado, los archivos para temas del datatable los ubicaremos dentro de la etiqueta `<head> de nuestro documento HTML`.

```html
<head>
  <!-- Más archivos CSS -->

  {!! Html::style('assets/css/datatable-bootstrap.css') !!}
</head>
```
De esta forma hemos conseguido crear nuestro primer DataTable.

# Crear un DataTable
Existen diversas formas de llenar un DataTable con datos, veremos algunas de las más usadas.

## Ocupando el DOM (etiqueta `<table>`)
Ocuparemos una tabla HTML con un id="example", posteriormente convertiremos la tabla en un DataTable mediante un script.

* HTML de la [tabla](../material/txt/tabla.txt)

El código Javascript para convertir la tabla con id="example" en un DataTable es:

```javascript
$(document).ready(function() {
    $('#example').dataTable();
} );
```

## Ocupando un dataset en Javacript
Un arreglo de arreglos en Javascript puede ser el dataset de un DataTable.

* Javascript del [dataset](../material/txt/javascript.txt)

Una vez definido el dataset bastará con ejecutar la siguiente función para crear una table en un div con id="demo" en nuestro HTML.

```javascript
$(document).ready(function() {
    $('#demo').html( '<table cellpadding="0" cellspacing="0" border="0" class="display" id="example"></table>' );

    $('#example').dataTable( {
        "data": dataSet,
        "columns": [
            { "title": "Engine" },
            { "title": "Browser" },
            { "title": "Platform" },
            { "title": "Version", "class": "center" },
            { "title": "Grade", "class": "center" }
        ]
    } );
} );
```

El código HTML es el siguiente:
```html
<html>
<head>
</head>
<body>
  <div id="demo">
  </div>
</body>
</html>
```

## Ocupando AJAX
Un DataTable puede leer los datos de una tabla de un json obtenido vía AJAX, el json puede ser servido mediante un Web Service o mediante un json.txt que lo contenga.

* El archivo [JSON](../material/txt/json.txt)

El código Javascript para la creación de DataTable ocupando el json.

```javascript
$(document).ready(function() {
    $('#example').dataTable( {
        "ajax": 'json.txt'
    } );
} );
```

El archivo HTML contiene una tabla con un id="example".
```html
<table id="example" class="display" cellspacing="0" width="100%">
        <thead>
            <tr>
                <th>Name</th>
                <th>Position</th>
                <th>Office</th>
                <th>Extn.</th>
                <th>Start date</th>
                <th>Salary</th>
            </tr>
        </thead>

        <tfoot>
            <tr>
                <th>Name</th>
                <th>Position</th>
                <th>Office</th>
                <th>Extn.</th>
                <th>Start date</th>
                <th>Salary</th>
            </tr>
        </tfoot>
    </table>
```

# Internacionalizar un dataTable
Es posible cambiar el idioma de las etiquetas de un datatable si bajamos el archivo de la traducción en el siguiente [link](https://www.datatables.net/plug-ins/i18n/).

Para ocuparlo tenemos que hacer referencia en la función de la creación del DataTable:

```javascript
<script type="text/javascript" src="jquery.dataTables.js"></script>
<script type="text/javascript">
    $(document).ready(function() {
        $('#example').dataTable( {
            "language": {
                "url": "dataTables.spanish.lang"
            }
        } );
    } );
</script>
```

# Filtrar datos de un DataTable
Podemos ocupar el campo de texto de búsqueda del DataTable y buscar bajo diferentes críterios separando por un espacio en blanco.

Ejemplo:
* Para buscar un administrador cuyo nombre empiece con la letra O y su teléfono sea extensión 228, podemos poner `administrador O 228`
