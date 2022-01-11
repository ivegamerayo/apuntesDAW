# Uso básico de Laravel

## Nuevo proyecto
Para la creacion de un proyecto es necesario tener composer y xampp, teniendo estos requisitos podemos ejecutar lo siguiente

```bash
composer create-project laravel/laravel pruebacontrolador --prefer-dist
```

**Las vistas**

Las vistas se encuentran en el directorio ```resources/views/``` es recomendable que si no sabemos que poner pongamos ```vista.blade.php```

En nuestro caso hemos creado el archivo ```resultado.blade.php```  y en él crearemos un html:5 y sacaremos por pantalla el resultado

```html
<!DOCTYPE html>

<html lang="en">

<head>

 <meta charset="UTF-8">

 <meta http-equiv="X-UA-Compatible" content="IE=edge">

 <meta name="viewport" content="width=device-width, initial-scale=1.0">

 <title>Document</title>

</head>

<body>

 {{$resultado}}

</body>

</html>
```


**Creacion de los controladores**

Dentro del directorio raiz del proyecto ejecutamos el comando

```bash
php artisan make:controller EjemploController
```

Es obligatorio que el nombre de los controladores tienen que escribirse CammelCase

Por ejemplo, hemos creado el anterior controlador al cual le hemos añadido lo siguiente

```php
<?php

  

namespace App\Http\Controllers;

  

use Illuminate\Http\Request;

  

class EjemploController extends Controller

{

 public function suma($num1, $num2){

 $res = $num1 + $num2;

  

 return view('resultado', array('resultado' => $res));

 }

}
```

**Rutas**

Las rutas se encuentran en el archivo ```routes/web.php``` 
En nuestro caso seguimos con el ejemplo de arriba, de forma que añadimos la siguiente linea

```php
Route::get('/suma/{num1}/{num2}', 'App\Http\Controllers\EjemploController@suma');
```

## Ejercicio1
Ejecute el controlador MiController metodo toupper, convierte el texto a mayusculas y el resutlado se visualice en la vista resultado.blade.php


### Resolución
Vamos a reutilizar el codigo de antes, asi que lo primero que haremos sera crear el controlador, para ello ejecutamos lo siguiente

```bash
php artisan make:controller MiController
```
De esta manera ya tendremos creado el controlador, ahora vamos a editarlo
```php
<?php

namespace App\Http\Controllers;

  

use Illuminate\Http\Request;

  

class MiController extends Controller

{

 function toupper($texto){

 $res = strtoupper($texto);

  

 return view('resultado', array('resultado' => $res));

 }

}
```

**Editamos las rutas**

Añadimos la siguiente ruta al final del archivo

```php
Route::get('/toupper/{texto}', 'App\Http\Controllers\MiController@toupper');
```

**Vista**

_Vista, si hemos hecho el ejercicio del ejemplo no tendremos que cambiar absolutamente nada_


## Ejercicio 2 (Formularios)

**Creamos un proyecto llamado _Sumar_numeros_ **

```bash
composer create-project laravel/laravel sumar_numeros --prefer-dist
```

**Generamos las vistas**
![[Pasted image 20211220095505.png]]

La pagina web ```show.blade.php```  tiene la siguiente estructura
```html
<!DOCTYPE html>

<html lang="en">

<head>

 <meta charset="UTF-8">

 <meta http-equiv="X-UA-Compatible" content="IE=edge">

 <meta name="viewport" content="width=device-width, initial-scale=1.0">

 <title>Document</title>

</head>

<body>

 La suma es: {{$resultado}}

</body>

</html>
```

La pagina web ```formulario.blade.php``` quedaria de la siguiente manera
```html
<!DOCTYPE html>

<html lang="en">

<head>

 <meta charset="UTF-8">

 <meta http-equiv="X-UA-Compatible" content="IE=edge">

 <meta name="viewport" content="width=device-width, initial-scale=1.0">

 <title>Document</title>

</head>

<body>

 <form action="hazlasuma" method="post">

 {{csrf_field()}}

 Numero1 <input type="text" name="numero1" id=""> <br><br>

 Numero2 <input type="text" name="numero2" id=""> <br><br>

 <input type="submit" value="Sumar">

 </form>

</body>

</html>
```

**OBLIGATORIO** la linea {{csrf_field()}} es obligatorio ponerlo justo debajo de la etiqueta form, es un control de errores para hackers.


**_OJO_**
A diferencia de normalmente en este caso dentro del _action_ no hemos puesto un php, este nombre nos lo podemos inventar pero luego tendremos que usarlo en las rutas.


**Generando el controlador**

Ejecutamos el siguiente comando

```bash
php artisan make:controller SumarController
```

Primera novedad, cuando un controlador recibe los datos usamos el objeto ```Request $request``` 


```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class SumarController extends Controller

{

 public function suma(Request $request){

 //obtengo los datos del formulario

 $n1 = $request->input('numero1');

 $n2 = $request->input('numero2');


 //Se podrian hacer als validaciones

 $res = $n1 + $n2;

 
 return view('show', array('resultado'=>$res));


 }

}
```

**Modificando las rutas**
```php
<?php


use Illuminate\Support\Facades\Route;
  

Route::get('/', function () {

 return view('formulario');

});

Route::post('/hazlasuma', 'App\Http\Controllers\SumarController@suma');
```
En este caso podemos ver que hemos modificando la ruta principal cambiando el welcome por formulario, aunque no es totalmente necesario se hace para que cuando inicies ya te lleve a el formulario.

La siguiente ruta la ponemos como post, el nombre de la ruta es la que escribimos en la vista del formulario como action. y lo siguiente es el controlador.

Laravel basic [[DWS/Laravel/Laravel basics]]