---
title: 'Tutorial Music Store MVC 3 en español &#8211; Vistas y modelos'
author: admin
layout: post
permalink: /tutorial-music-store-mvc-3-en-espanol-vistas-y-modelos/
dsq_thread_id:
  - 
categories:
  - Artículos
---
# 

Esta entrada es parte y continuación del articulo 

[![Vistas y modelos][2]][2]

Vistas y modelos[RobertFrancis][2] /[Free Photos][3]

Continuaremos generando HTML como respuesta para los navegadores.

 []: http://foter.com/photo/view-master/ "View Master"
 [2]: http://www.flickr.com/photos/57001982@N00/
 [3]: http://foter.com/ "Free Photos"

Añadiendo una plantilla de vista

Para usar una vista debemos de cambiar el método index de nuestro Homecontroller para que regrese una acción (ActionResult) y esta acción regrese una vista:

    public class HomeController : Controller
    {
    // GET: /Home/
    public ActionResult Index()
    {
    return View();
    }
    }
    

Ahora añadiremos una vista a nuestro proyecto. Para hacer esto nos colocamos sobre el nombre del método index y le damos click derecho y pulsamos “Add View”.

Los datos precargados de esta ventana están bien así que solo confirmamos pulsando el botón Add.

Después de esto se creara un archivo llamado Index.cshtml en el directorio /views/home esta acción crea el folder si no existiera.

El nombre del folder y su ubicación son importantes ya que siguen las convenciones usadas en MVC.

MVC permite mandar a llamar el template que tiene el mismo nombre que el método que se llama.

Usando elementos comunes como plantilla

La mayoría de los sitios comparten contenido como menú, imágenes de logo, hojas de estilo, etc.  El motor razor de plantillas hace que esto fácil de manejar usando una página llamada _layout.cshtml que automáticamente es creada en /views/shared folder.

Dentro de ese archivo nos encontramos con este código:

    
    
    
    
    
    
    
    
    
    @ViewBag.Title
    
    
    
    
    
    
    
    
    
    
    
    @RenderBody()
    
    
    
    
    
    

El contenido de nuestra vista individual será desplegada por el comando @renderbody() y cualquier contenido común que queramos mostrar fuera puede ser añadido dentro del marcado del archivo _layout.cshtml. Nosotros queremos tener un encabezado común con enlaces a nuestra página de portada y nuestra tienda en cada una de nuestras páginas dentro del sitio, así que las añadiremos a la plantilla encima de la declaración @renderbody.

    
    
    
    
    
    
    ASP.NET MVC MUSIC STORE
    
    
    
    Home
    
    Store
    
    
    
    
    
    @RenderBody()
    
    
    
    

Actualizando la hoja de estilos

El proyecto vacio incluye un archivo css fluido que contiene estilos para desplegar mensajes de validación.

Para este tutorial podemos bajarnos la aplicación completa de esta dirección [http://mvcmusicstore.codeplex.com][4] dentro de la carpeta assets de la solución encontraremos una carpeta llamada contents esa literalmente podemos reemplazarla por la que nosotros tenemos ya que esta contiene imágenes y css con estilos creados.

 [4]: http://mvcmusicstore.codeplex.com/

Si corremos ahora la aplicación podremos ver la aplicación de los estilos.

Lo que sucedió fue lo siguiente:

El método index del HomeController encontró y desplego el archivo /views/home/index.cshtml debido a la línea “return view()” porque el archivo siguió la convención estandard a la hora de nombrarlo.

La portada esta desplegando el mensaje que está definido en el archivo /views/home/index.cshtml.

La página de inicio esta usando la plantilla _layout.cshtml, y el mensaje de bienvenida está contenida  dentro del sitio standard de html.

Usando un modelo para pasar información a nuestra vista

Una vista mostrara código html plano, para crear un sitio dinámico debemos buscar pasar información desde nuestro controlador a nuestras vistas.

En el modelo vista controlador, el termino modelo se refiere a objetos que representan los datos en la aplicación.  A menudo, los objetos modelo corresponden a tablas de nuestra base de datos, pero no tiene que ser así.

Los métodos de acción del controlador que retornan el resultado de la acción pueden pasar un objeto modelo a la vista.  Esto permite a un controlador empaquetar limpiamente toda la información necesaria para generar una respuesta, y entonces pasar esta información a la plantilla de la vista para usarse y generar una apropiada respuesta html.

Primero que todo crearemos algunas clases modelo que representan Géneros y Álbumes dentro de nuestra tienda. Crearemos primero la clase género. Click derecho sobre el folder “Models” dentro de nuestro proyecto, escogiendo la opción “Add Class”, y nombramos nuestro archivo “Genre.cs”.

Agregamos  una propiedad pública llamada “Name” de tipo cadena en la clase que acabamos de crear.

    public class Genre
    
    {
    
    public string Name { get; set; }
    
    }
    
    

Nota: la notación { get; set; } es usada por C# para auto-implementar propiedades. Esta nos da los beneficios de una propiedad sin necesidad de declararlos antes.

Seguidamente seguimos los mismos pasos para crear la clase para álbumes “Album.cs” esta contiene titulo y genero como propiedad.

    public class Album
    
    {
    
    public string Title { get; set; }
    
    public Genre genre { get; set; }
    
    }
    

Ahora modificaremos el StoreController para usar vistas que desplieguen información dinámica desde nuestro modelo.

Por motivos de demostración – nombraremos nuestros álbumes basados en el ID solicitado, podremos desplegar información en la vista siguiente: –imagen–

Comenzaremos cambiando la acción  detalle de la tienda ahora eta mostrara información para un álbum.

Añadimos una declaración de uso al inicio de la clase StoreControllers para incluir los modelos de la tienda.

    using System;
    
    using System.Collections.Generic;
    
    using System.Linq;
    
    using System.Web;
    
    using System.Web.Mvc;
    
    using MvcMusicStore.Models;
    
    

Ahora actualizamos la acción detalles del controlador para que retorne una acción (ActionResult)  en lugar de una cadena, como hicimos con el método index del Homecontroller.

public ActionResult Details(int id)

Ahora podremos modificar la lógica que retorna el objeto Álbum a la vista. Después de este tutorial podremos recibir la información desde la base de datos, pero por ahora trabajaremos con “datos de prueba” para empezar.

    public ActionResult Details(int id)
    
    {
    
    var album = new Album { Title = "Album "   id };
    
    return View(album);
    
    }
    
    

Nota: las variables dentro de nuestro álbum no son variables de tiempo de ejecución.

El compilador de c# usa tipos de inferencia basados en lo que asignamos a la variable para determinar qué tipo de álbum es y compilar la variable local de álbum como un tipo  “álbum”, por eso podemos obtener un tiempo de compilado controlado y soporte del editor de código de visual estudio.

Ahora crearemos una vista que usa nuestro Álbum para generar una respuesta html. Antes de seguir debemos compilar nuestro proyecto para que la vista pueda reconocer la clase Álbum que acabamos de crear. Podemos construir con las teclas ctrl-shift-b o desde el menú build.

Ahora debemos dar click derecho sobre el método Details y seleccionar “Add View” del menú contextual.

Crearemos una vista como lo hicimos con el HomeController. Pero como lo estamos creando en el control StoreController este será generado dentro en el archivo /view/store/index.cshtml.

A diferencia del anterior ahora seleccionaremos la opción “Create a strongly-typed” o “vista fuertemente tipeada”.  Vamos a seleccionar nuestra clase “Álbum” dentro de la vista “data-class” o “vista de clase-datos” drop-downlist. Esto es porque el dialogo para crear nuestra plantilla espera que un objeto álbum sea pasado y usado.

![Añadir vista Razor cshtml][5]
Añadir vista Razor cshtml

Esto nos creara el archivo /views/store/details.cshtml conteniendo este código:

 [5]: http://www.eliezerdiaz.com/wp-content/uploads/2011/09/039-300x295.jpg "Añadir vista Razor cshtml"

    @model MvcMusicStore.Models.Album
    
    @{
    
    ViewBag.Title = "Details";
    
    }
    
    Details
    
    

La primera línea indica que esta clase es fuertemente tipeada de tipo Álbum. El motor de razor de plantillas entiende que le pasaremos un objeto Álbum, así que podremos fácilmente acceder al modelo de sus propiedades e incluso tener el beneficio de “Intelli$ense” en nuestro editor de Visual Studio.

Modificaremos la etiqueta  par que despliegue la propiedad que contiene el  titulo del álbum:

    Album:@Model.Title
    
    

La palabra @Model reconoce las propiedades y métodos que la clase álbum soporta, esto es Intelli$ense.

Ahora volvamos a correr nuestro proyecto y visitemos la url /store/details/5. Veremos el detalle del álbum.

Ahora actualizaremos de manera similar el método Browse de nuestra tienda. Modificamos el método para que retorne un “ActionResult”, y modificaremos la lógica del método así que crearemos un objeto de Género y lo devolveremos a la vista.

Actualicemos los elementos de la etiqueta  en la vista código (/views/store/browse.cshtml) para desplegar la información del género.

Corramos nuestra aplicación y entremos a la dirección /Store/Browse?Genre=Disco.

Ahora modifiquemos nuestro método de acción “Index” en store y veremos una lista de todos los géneros en nuestra tienda. Haremos esto usando una lista de géneros como nuestro objeto modelo, que será solamente una lista de géneros.



Ahora le creamos una vista, haciendo click derecho en el método index y añadiendo una vista que reciba de modelo la clase Genre.

De primero debemos cambiar la declaración del modelo para indicar que nuestra vista esperara una lista de de objetos tipo Genre, mas que solo un objeto. Cambiamos la primera línea a:

Para decirle al motor razor de plantillas que este estará trabajando con un modelo de objetos que contiene varios objetos Género, usamos ienumerable ienumerable en lugar de List< Genre>  pues estas son colecciones genéricas, lo que permite que usemos nuestro tipo de modelo  después de cualquier otro tipo de objeto que soporte la interfaz ienumerable.

Ahora lo que haremos es recorrer nuestra lista de objetos:



Notaremos que tenemos soporte total para Intelli$ense mientras escribimos nuestro código, cuando tipiamos “@Model.” vemos todos los métodos y propiedades soportadas por un IEnumerable de tipos genero.

[![los métodos y propiedades soportadas por un IEnumerable de tipos genero][7]][7]
los métodos y propiedades soportadas por un IEnumerable de tipos genero

Dentro de nuestro ciclo “foreach”, visual web developer conoce que a cada item de  nuestra iteración es de tipo género.

 []: http://www.eliezerdiaz.com/wp-content/uploads/2011/09/045.jpg

![ciclo foreach de una estructura IEnumerable ][7]
ciclo foreach de una estructura IEnumerable

Ahora, las opciones mostradas en el objeto genero y determinadas por cada iteración tienen una propiedad nombre y esta es mostrada. Esto genera también links para edición, detalle y eliminación para cada uno de los ítems.

 [7]: http://www.eliezerdiaz.com/wp-content/uploads/2011/09/046-300x139.jpg "ciclo foreach de una estructura IEnumerable "

Tomaremos ventaja de esto posteriormente cuando hagamos la parte de administración de nuestra tienda, pero por ahora la simple lista está bien.

Si corremos nuestra aplicación entrando a /store, veremos que nuestro conteo y nuestra lista es desplegada.

Añadiendo enlaces entre páginas

Nuestro enlace a /store lista los nombres de los géneros como simple texto, cambiemos esto colocando el enlace correspondiente a /store/browse, para eso modificamos nuestro genero “Funky Disco” que irá a /store/browse?genre=Funky Disco. Modificaremos la plantilla /views/store/index.cshtml para que muestre los enlaces de este modo:. **(No escribas esto en tu código, te mostrare como hacerlo mejor. )**



Este código funciona, pero es un problema dejarlo como código duro, si nosotros cambiamos el nombre del control tendremos que hacer muchas modificaciones para hacerlo funcionar como queremos.

La alternativa a esto es el uso del método helper html que provee MVC que están disponibles desde nuestra plantilla para mostrar una variedad de tareas como esta. El método helper Html.ActionLink()  es para hacer esto, hace fácil construir enlaces HTML y tiene cuidado de no molestarnos con molestos detalles a la hora de hacer rutas en la codificación de la URL.

html.ActionLink() tiene diferentes formas de especificar la información que se necesitan en los links. En un caso simple, le enviamos el texto que tendrá el enlace y el método de acción al que nos queremos dirigir. Por ejemplo si nos queremos dirigir a “/store/” al método Index() de la pagina de detalles con el texto “Go to the Store Index” usamos:

@Html.ActionLink(“Vamos al inicio de la tienda”, “Index”)

En este caso no especificamos el controlador al que nos dirigimos ya que vamos a otra acción dentro del mismo controlador.

Nuestros enlaces dentro de la página de navegación necesitan un parámetro, entonces usamos una opción de  del método HTML.ActionLink con tres parámetros.

1.  Texto del enlace, que desplegara el nombre del genero
2.  Nombre de la acción del controlador (browse)
3.  parámetro de valores de ruta, especificando el género y el valor del nombre del valor

Poniendo todo junto esto es lloque deberemos de escribir:

Ahora podremos correr nuestro proyecto y veremos cómo cada género es desplegado como un enlace que nos llevara a la dirección /store/browse?genre=[genre].

el htm luce así: