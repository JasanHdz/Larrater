# Curso de Php con Laravel

## ¿Porque laravel?
Laravel está orientado a apliaciones de desarrollo rápido, eso significa que tiene un monton de código ya implementado para nosotros y non necesitamos hacer tanto esfuerzo para tener un montón de funcionalidades muy modernas.
**Por ejemplo:**
- Notificaciones en tiempo real: donde conectamos una aplicación backend como **laravel** con un frontend como **vue.js** con laravel-eco.
- Login con redes sociales, que quizas sería un montón de configuración del lado de la aplicación y que **laravel-socialite** nos ahorra casí todo el trabajo.

## Installación

Lo que yo recomiendo para usuarios windows es descargar un entorno Todo en uno, que incluye php, mysql, apache, y otros servicios, en esté curso estaremos ocupando **XAMPP**. 
Una vez instalado xampp, procedemos a instalar composer, visitando la página oficial de composer y seguir las instrucciones.

Ahora que ya tenemos composer instalado lo que vamos a hacer es ir a la carpeta de **htdocs** que es la carpeta que usa xampp para entrar al servidor.

Nos colocaremos en esa ruta desde la Terminal y instalaremos Laravel de forma global dentro de composer con el siguiente comando. ```composer global require "laravel/installer"```.

Esto nos instalará **Laravel** de forma Global. Una vez instalado podremos crear proyectos Laravel en la carpeta que quieramos.

## Iniciar proyecto con Laravel

Para iniciar un nuevo proyecto en Laravel lo que tenemos que hacer es correr el comando en terminal: 
```php
# Indicamos que queremos crear un proyecto laravel nuevo y le pasamos un Nombre, que será la carpeta donde instalará todo el entorno laravel.
laravel new "NombreProyecto"
```

## Inicia el Proyecto de Laravel

Comenzaremos lanzando nuestro servidor de larevel y para inicializar el servidor utilizamos el comando “php artisan serve”
```php
php artisan serve
```
Laravel nos genera todos los archivos necesarios para nuestro web app ahora seguiremos construyendo nuestra aplicación, modificaremos nuestros archivos de rutas y cómo mostrar html en laravel, comenzaremos editando el archivo web.php aquí observamos que tenemos en view: welcome, este es el archivo que se nos mostró al entrar a localhost:8000 y lo podemos editar para ver los cambios.

## Agrega Rutas en el Controller y Templates de Blade

Ahora regresaremos a nuestro archivo web.php aprenderemos a agregar secciones de nuestro sitio implementando una página de “acerca de nosotros” en localhost:8000/acerca donde podemos mostrar información acerca de nuestra compañía o sitio web.
```php
Route::get('/about', function () {
return 'acerca de nosotros';
});
```
Ahora generaremos una vista para mostrar en la nueva sección de nuestro sitio recién creada, con esto vamos a crear una nueva vista y la haremos copiando el contenido de nuestro archivo welcome.blade.php y lo modificaremos apra crear nuestra vista “acerca de nosotros”.

## Blade: usando repeticiones y condicionales

Ahora veremos instrucciones para crear controllers y además instrucciones de flujo con BLADE cambiando los enlaces y la forma en la que los estamos generando.

La sintáxis ``{{ var }}`` equivale al comando echo de PHP.

La sintáxis para declarar un if:
```php
@if (isset($teacher))
                <p>Profesor: {{ $teacher }} </p>
                @else
                <p>Profesor a definir</p>
                @endif
```
Sintáxis para declarar un **ForEach** en blade:
```php
  @foreach ($links as $link => $text)
    <a href=" {{ $link }} " target="_blank"> {{$text}} </a>
  @endforeach
```
Debemos de Recordar que los valores que lee blade deben ser definidos y pasados por párametro dentro de un objeto donde creamos la ruta. **ejemplo:**
```php
Route::get('/', function () {
    $links= [
        'https://platzi.com/laravel' => 'Curso de Laravel',
        'https://laravel.com' => 'Página de laravel',
    ];
    return view('welcome', [
        'links' => $links,
        'teacher' => 'Guido Contreras Woda'
    ]);
});
```
## Controllers

los controllers son clases y objetos que vamos a usar para modelar los problemas a resolver, puntualmente resolver el problema de recibir iun pedido de la red y dar una respuesta.

Vamos a mejorar nuestra aplicación llevándonos el código que hicimos en el archivo de rutas hacia los controllers.

Esto lo haremos utilizando artisan primero recuerden tener activo su servidor y en otro servidor vamos a ejecurar el comando:
```php
php artisan make:controller --help
```
y aquí le pediremos ayuda para saber que argumentos requiere.

Ahora procederemos a crear nuestro controller utilizando php artisan make:controller PagesController y será creado exitosamente por último procederemos a mover las direcciones a nuestro controlador.

### Creando Controllador

En la terminal ejecutaremos el comando php artisan make:controller seguido del nombre que recibirá nuestro controlador.

Cuando el controlador hallá sido creado, lo que haremos es ir a la dirección ``Http\Controllers\PagesController.php`` que es el archivo que creamos desde la terminal.

Este PagesController no es más que una clase que será el controllador y dentro de ella, tendrá métodos que realizarán una *acción* cuando sean llamados en el **route**. Ejemplo:
```php
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;

class PagesController extends Controller
{
    public function getHome() {
        $links= [
            'https://platzi.com/laravel' => 'Curso de Laravel',
            'https://laravel.com' => 'Página de laravel',
        ];
        return view('welcome', [
            'links' => $links,
            'teacher' => 'Guido Contreras Woda'
        ]);
    }

    public function About() {
        return view('about');
    }
}
```

### Modificando route

Vamos a llevarnos la function que estámos realizando y vamos a llevarla al controlador, ahí es donde vamos a poner la lógica necesaria para responder al llamado de la ruta.

En la ruta unicamente le pasaremos, como párametro el nombre del controlador seguido de su action, ejemplo:
```php
Route::get('/', 'PagesController@getHome');
Route::get('/acerca', 'PagesController@About');
```
## Crea Layouts en Blade para Evitar la Duplicación de Código

Ahora vamos a aparender a crear Layouts para poder crear pagínas nuevas fácilmente, reutilizando código que nuestras páginas tendrán en común usaremos 3 comandos principales Layout files @extends @yield @section.

para crear templates o un html padre(layout) lo que vamos a hacer es crear una nueva carpeta, donde iremos poniendo los **layouts** que vamos a ir creando.

Dentro de la carpeta crearemos nuestro primer layout que se llamara: **app.blade.php** esté archivo contendra todo el html que queremos heredar, y donde el contenido sea modificado se colocarán **yield** para indicar que esa section podrá ser modificada.

Ahora en nuestro archivo que queremos mezclar con el layout, lo que haremos será:
1. Indicar que el archivo no está solo, lo que haremos será decir que el archivo hereda de otro con la palabra reservada ``@extends('name')``.
2. Agregaremos el HTML que queremos modificar o incluir envolviéndolo el las etiquetas 
```
@section('content') 
<Html>HTML que queremos incluir</Html>
@endsection
``` 
3. Una vez puesto estó podemos revisar que la mezcla de html se hallá realizado correctamente, de no ser así revisar las etiquetas @yield y @extends tengan los párametros correctos.

### ¿Qué hacer si quiero modificar una linea dinámicamente?

Para modificar una linea dinámicamente puedo hacer qué está linea sea modificado usando la etiqueta @yield, y a la vez puedo darle un valor por defecto en caso de que el valor no sea modificado o agregado.
Ejemplo:
```php
# En esté ejemplo lo haremos con la linea <title></title>
<title>@yield('title', 'Laratter by Platzi') </title>
```
- Como primer párametro le indicaremos el nombre de la section que tendrá este campo, y como segundo párametro le pondremos un valor por defecto en caso de no ser modificado.

- Si deseo modificarlo solo debo especificarlo en mi archivo. Y si no lo hago el valor por defecto se heredará.

## Integrando Boostrap 

Ahora agregaremos Bootstrap a nuestro proyecto para poder acelerar el proceso de desarrollo, en la sección de enlaces puedes acceder a la versión qu estaremos utilizando.

Vamos a empezar a desarrollar nuestro proyecto, vamos a borrar todo el contenido default de nuestro sitio y vamos a comenzar a diseñar la página principal de nuestro sitio.

1. Vamos a entrar a la página de **boostrap** y entraremos en la sección de descargas, ahi copiaremos el CDN, el que contrendrá un link y un script. El link lo pegaremos en el head, mientras que el script lo pondremos antes de cerrar la etiqueta **body**.
2. Entraremos al template de nuestro welcome.blade y pondremos el siguiente código
```html
<div class="jumbotron text-center">
    <h1>Laratter</h1>
    <nav>
        <ul class="nav nav-pills">
            <li class="nav-item">
                <a class="nav-link" href="/">Home</a>
            </li class="nav-item">
            <li>
                <a class="nav-link" href="/acerca">Acerca de Nosotros</a>
            </li>
        </ul>
    </nav>
</div>
```
3. Ahora crearemos contenido dinámico usando images para mostrar en nuestro welcome.

Primero crearemos un *array* de nombre **messages** dentro de nuestro controlador antes de retornar la vista.
Este array va tener 4 objetos por ahora, con los valores de: **id, content e image.** 
```php
$messages = [
            [
                'id' => 1,
                'content' => 'Esté es mi primer mensaje',
                'image' => 'https://source.unsplash.com/category/nature/600x338?4',
            ],
            [
                'id' => 2,
                'content' => 'Esté es mi segundo mensaje',
                'image' => 'https://source.unsplash.com/category/nature/600x338?1',
            ],
            [
                'id' => 3,
                'content' => 'Esté es mi tercer mensaje',
                'image' => 'https://source.unsplash.com/category/nature/600x338?2',
            ],
            [
                'id' => 4,
                'content' => 'Otro mensaje más mensaje',
                'image' => 'https://source.unsplash.com/category/nature/600x338?3',
            ]
        ];
``` 
Las images serán requeridas de una api llamada [Unsplash](https://gersonlazaro.com/unsplash-api-miles-de-fotos-gratis-en-tu-sitio-web-o-aplicacion/).

4. Ahora en la vista vamos a crear una Fila donde vamos a mostrar los elementos de los objetos que creamos en la variable **message** 
5. Adentro del la fila crearemos un forEach para iterar los elementos del objeto e imprimir su valores del siguiente manera.
```html
<div class="row">
    @forelse ($messages as $message)
        <div class="col-6">
            <img class="img-thumbnail" src=" {{$message['image']}} " alt="">
            <p class="card-text">
                {{ $message['content'] }}
                <a href="/messages/{{ $message['id'] }}">Leer más</a>
            </p>
        </div>
    @empty
        <p>No hay mensajes destacados!</p>        
    @endforelse
</div>
```
- **ForElse** es un Foreach que tendra un else cuando no encuentre elementos dentro del array.
- **Empty** Nos ayudará a mostrar un mensaje alternativo cuando el objeto venga vacío
- Al final debemos cerrar el ForElse usando **endforelese**.

6. Ahora vamos a retornar la vista pasandole como primer párametro la vista y como segundo párametro la información que va a requerir, donde pondremos la el arreglo $messages que creamos de la siguiente manerá.
```php
return view('welcome', [
            'messages' => $messages,
        ]);
```
7. Por ultimo eliminaremos el elemento **Acerca de nosotros** porque no la ocuparemos, así que eliminaremos: la vista, controlador y la ruta para que ya no exista más.







