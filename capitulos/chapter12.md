# Validaciones en Laravel
Existen varias formas de validar nuestra aplicación para cubrir aspectos de seguridad como SQL Injection, ataques XSS o CSRF, algunas de ellas son:
* Validación de lado del cliente (Javascript y etiquetas HTML).
* Validación a nivel de base de datos (Migraciones y modelos).
* Validación de formularios (Request).

## Validación del lado del cliente:
Podemos validar que los campos de un formulario sean requeridos al agregar el atributo `required`.

```html
<form action="demo_form.asp">
 Username: <input type="text" name="username" required>
 <input type="submit">
</form>
```

El atributo  `reqired` es un atributo booleano.
Cuando esta presente, este especifica que un campo debe ser rellenado antes de ser enviado el contenido del formulario.

El atributo `required` trabaja con los siguientes tipos de input:
* text
* search
* url
* tel
* email
* password
* data pickers
* number
* checkbox

#### El atributo *pattern*
Con se menciono anteriormente, con required solo se necesita de cualquier valor en el elemento `<input>` para ser válido, pero utilizando el atributo pattern en conjunto, se logra que se verifique no solo la presencia de un valor, sino que este valor debe contener un formato, una longitud o un tipo de dato especifico. Esto último se logra definiendo un patrón con expresiones regulares.

```html
<label for="tel">Teléfono 10 dígitos empezando por 228</label>
<input type="text" pattern="^228\d{8}$">
```

Para utilizar el atributo pattern es recomendable utilizar el type="text" y no un type de los predefinidos en HTML5 que ya cuentan con patrones de validación en el propio navegador. Mezclar ambos puede llevar a resultados inesperados.

#### Validación de formularios con plugins JQuery
El mejor plugin JQuery para validar formularios es [formvalidation](http://formvalidation.io/). Sin embargo formvalidation es un plugin que tiene un costo dependiendo la licencia que queramos ocupar.

Formvalidation es compatible con los formularios de los frameworks css más populares:
* Bootstrap
* Foundation
* Pure
* Semantic UI
* Otros

[Smoke](http://alfredobarron.github.io/smoke/#/) es el más completo plugin JQuery diseñado para trabajar con bootstrap 3, además es open source e incluye las siguientes características:
* [Validación de formularios](http://alfredobarron.github.io/smoke/#/validate).
* [Sistema de notificaciones](http://alfredobarron.github.io/smoke/#/notifications).
* [Progressbar](http://alfredobarron.github.io/smoke/#/progressbar).
* [Soporte para fullscreen](http://alfredobarron.github.io/smoke/#/fullscreen).
* [Agregar funcionalidad extra a los paneles de bootstrap](http://alfredobarron.github.io/smoke/#/panel).
* [Helpers para conversión de tipos](http://alfredobarron.github.io/smoke/#/helpers).

Para incluir smoke en nuestro proyecto sólo tenemos que descargar el plugin de [aquí](http://alfredobarron.github.io/smoke/#/getting-started).

Extraer los archivos del zip, y colocar los archivos CSS y JS dentro de la carpeta public/assets de nuestro proyecto en Laravel:

```
public/
  assets/
    css/
      smoke.css
      smoke.min.css
    js/
      smoke.js
      smoke.min.js
    lang/
      es.js
      es.min.js
```

Una vez hecho esto, debemos hacer referencia a los estilos y scripts desde nuestro documento html en la sección de head y body:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Ejemplo Smoke</title>
    {!! Html::style('assets/css/smoke.css') !!}
  </head>
  <body>

  {!! Html::script('assets/js/smoke.js') !!}
  {!! Html::script('assets/lang/es.js') !!}
  </body>
</html>
```

Los ejemplos sobre validaciones, notifiaciones, progressbar, etc los encontrarás en la página oficial de [Smoke](http://alfredobarron.github.io/smoke/index.html#/).

##Validación del lado del servidor (Request).
Laravel permite validar los datos enviados por un formulario de forma muy sencilla ocupando un Mecanismo llamados "Requests". Veamos un ejemplo ocupando el controller PastelesController visto en el [Anexo C. CRUD con Laravel](../anexos/crud.md) para comprender su funcionamiento:

Lo primero que debemos hacer es crear un request para el método store de PastelesController ya que necesitamos validar que los datos enviados en el formulario para crear un nuevo pastel sean válidos.

Ejemplo:
```php
php artisan make:request CrearPastelesRequest
```

Con este comando Crearemos el Request CrearPastelesRequest ubicado en:
```
app/Http/Request/CrearPastelesRequest
```

Su contenido es el siguiente:
```php
<?php

namespace Curso\Http\Requests;

use Curso\Http\Requests\Request;

class CrearPastelesRequest extends Request
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return false;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            //
        ];
    }
}
```

Lo siguiente que haremos será cambiar el valor que regresa el método **authorize()** de false a true para permitir que el Request lo pueda ocupar cualquier usuario.

```php
public function authorize()
{
    return true;
}

```

Posteriormente en el método **rules()** agregaremos las reglas de validación del formulario para crear pasteles quedando así:

```php
public function rules()
   {
       return [
           'nombre' => 'required|string|max:60',
           'sabor' => 'required|in:chocolate,vainilla,cheesecake'
       ];
   }
```

Al final el archivo CrearPastelesRequest deberá verse así:
```php
<?php

namespace Curso\Http\Requests;

use Curso\Http\Requests\Request;

class CrearPastelesRequest extends Request
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'nombre' => 'required|string|max:60',
            'sabor' => 'required|in:chocolate,vainilla,cheesecake'
        ];
    }
}

```

De igual forma crearemos el Request para el método update de PastelesController.

```php
php artisan make:request EditarPastelesRequest
```

Y el archivo lo dejaremos como se muestra a continuación.

```php
<?php

namespace Curso\Http\Requests;

use Curso\Http\Requests\Request;

class EditarPastelesRequest extends Request
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'nombre' => 'required|string|size:60',
            'sabor' => 'required|in:chocolate,vainilla,cheesecake'
        ];
    }
}
```

Las reglas de validación ocupadas en el ejemplo anterior las podemos encontrar explicadas con mayor detalle en la página de [Validaciones](http://laravel.com/docs/5.1/validation#rule-integer) de Laravel.

Para ocupar el nuevo request en PastelesController debemos incluirlo:

```php
use Curso\Http\Requests\CrearPastelesRequest;
use Curso\Http\Requests\EditarPastelesRequest;
```

En el modelo Pastel agregaremos una propiedad $fillable para indicar que atributos de la tabla pasteles podrán ser ocupados con el método
`$request->all()`.

El modelo pasteles quedaría así:
```php
<?php

namespace Curso;

use Illuminate\Database\Eloquent\Model;

class Pastel extends Model
{
    protected $table = 'pasteles';
    protected $fillable = ['nombre','sabor'];

    public function scopeSabor($query, $sabor){
        return $query->where('sabor',$sabor);
    }

    public function scopeId($query, $id){
        return $query->where('id',$id);
    }
}

```

Los métodos store y update recibirán como parametro un objeto Request para aplicar las reglas de validación, al final el controlador PastelesController debe tener el siguiente aspecto:
```php
<?php

namespace Curso\Http\Controllers;

use Illuminate\Http\Request;

use Curso\Http\Requests;
use Curso\Http\Controllers\Controller;

use Curso\Pastel;
use Curso\Http\Requests\CrearPastelesRequest;
use Curso\Http\Requests\EditarPastelesRequest;

class PastelesController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return Response
     */
    public function index()
    {
        $pasteles = Pastel::get();
        return view('pasteles.index')->with('pasteles', $pasteles);
    }

    /**
     * Show the form for creating a new resource.
     *
     * @return Response
     */
    public function create()
    {
        return view('pasteles.create');
    }

    /**
     * Store a newly created resource in storage.
     *
     * @return Response
     */
    public function store(CrearPastelesRequest $request)
    {
        $pastel = Pastel::create($request->all());
        return redirect()->route('pasteles.index');
    }

    /**
     * Display the specified resource.
     *
     * @param  int  $id
     * @return Response
     */
    public function show($id)
    {
        //
    }

    /**
     * Show the form for editing the specified resource.
     *
     * @param  int  $id
     * @return Response
     */
    public function edit($id)
    {
        $pastel = Pastel::find($id);
        return view('pasteles.edit')->with('pastel',$pastel);
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  int  $id
     * @return Response
     */
    public function update(EditarPastelesRequest $request, $id)
    {
        $pastel = Pastel::find($id);
        $pastel->fill($request->all());
        $pastel->save();
        return redirect()->route('pasteles.index');
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  int  $id
     * @return Response
     */
    public function destroy($id)
    {
        $pastel = Pastel::find($id);
        $pastel->delete();

        Pastel::destroy($id);
        return redirect()->route('pasteles.index');

    }
}
```
