# Nuevo proyecto
Para la creacion de un proyecto es necesario tener composer y xampp, teniendo estos requisitos podemos ejecutar lo siguiente

```bash
composer create-project laravel/laravel pruebacontrolador --prefer-dist
```

# Las vistas

Las vistas se encuentran en el directorio ```resources/views/``` es recomendable que si no sabemos que poner pongamos ```vista.blade.php```

# Ubicacion de las imágenes
Para ello vamos a crear una carpeta dentro de **public** llamada **images**

y luego desde las vistas lo llamamos de la siguiente forma 
```html
<img src="./images/imagen.png" alt="" />
``` 
Como podemos apreciar no hemos metido la ruta real si no que hemos trabajado como si estuviese ahí.

# Validaciones
## En el controlador
```php
//Hacemos el isset
if(!$request->has('a') || !$request->has('b') || !$request->has('c')){
	return view('resultado', ['error => "Faltan resultados"']);
} else {

//codigo del ejercicio anterior

return view('resultado', ['solucion1' => $sol1, 'solucion2'=>sol2]);

}
```

## En las vistas
```html

@if(isset($error))
<h1>{{$error}}</h1>
@else

Solucion1 {{$solucion1}}
Solucion2 {{$solucion2}}
```
