---
title: Tutorial Music Store MVC 3 en español – Acceso a los datos
author: admin
layout: post
permalink: /tutorial-music-store-mvc-3-en-espanol-acceso-a-los-datos/
dsq_thread_id:
  - 
categories:
  - Artículos
---
# 

![Acceso a los datos][1]

Acceso a los datos[adesigna][2] /[Free Photos][3]

Hasta ahora hemos estado enviando “información de prueba” desde nuestros controladores a nuestras vistas. Pero ahora estamos listos para montar una base real. En este tutorial estaremos cubriendo como usar SQL Server Compact Edition (comúnmente llamado) SQL CE el cual es gratis, integrado, basado en archivos que no requiere ninguna instalación o configuración, que lo hace ideal para desarrollo local.  
El acceso de base de datos con la entidad de marco de desarrollo codifica primero  
Se incluye soporte para la entidad de marco de desarrollo (EF) incluido en el proyecto MVC 3 para consultar y actualizar la base de datos. EF es un api de mapeo de objetos relacionales (ORM) que permite a los desarrolladores consultar y modificar los datos almacenados en la base de datos orientado a objetos.  
El Entity Framework versión 4 soporta un paradigma de desarrollo llamado code-first. Code-first permite crear el objeto modelo escribiendo clases simples (conocidas como POCO que viene de los objetos CLR “plain old” ), y pueden incluso crear la base de datos al vuelo desde tus clases.  
Cambios en nuestro modelo de clases  
Cambiaremos nuestro Modelo de clases  
Dejaremos la creación de la base de datos a la entidad del marco de desarrollo en este tutorial. Antes de hacer eso, aunque haremos unos cambios menores a nuestro modelo de clases para añadir algunas coas que usaremos más adelante.  
Añadiendo el modelo de clase Artista  
Nuestros álbumes deberán de asociarse con artistas, estará bien que añadamos un modelo simple de clase que describa a un artista. Añadimos una nueva clase a nuestro folder de modelos llamándolo Artist.cs y usemos el código mostrado abajo.

 [1]: http://eliezerdiaz.com/wp-content/uploads/2012/02/316-avz-database.jpg "Acceso a los datos"
 [2]: http://www.flickr.com/photos/35723892@N00/
 [3]: http://foter.com/ "Free Photos"

    namespace MvcMusicStore.Models
    {
        public class Artist
        {
            public int ArtistId { get; set; }
            public string Name { get; set; }
        }
    }

Actualizando nuestro modelo de clases  
Actualiza la clase álbum como se muestra abajo.

    namespace MvcMusicStore.Models
    {
        public class Album
        {
            public int      AlbumId     { get; set; }
            public int      GenreId     { get; set; }
            public int      ArtistId    { get; set; }
            public string   Title       { get; set; }
            public decimal  Price       { get; set; }
            public string   AlbumArtUrl { get; set; }
    
            public Genre    Genre       { get; set; }
            public Artist   Artist      { get; set; }
        }
    }
    

Ahora hagamos los siguientes cambios en la clase Genre

    using System.Collections.Generic;
     namespace MvcMusicStore.Models
    {
        public partial class Genre
        {
            public int      GenreId     { get; set; }
            public string   Name        { get; set; }
            public string   Description { get; set; }
            public List&lt;Album&gt; Albums   { get; set; }
        }
    }
    

Añadiendo el folder App_Data  
Cuando Añadimos un directorio App\_Data a nuestro proyecto mantenemos nuestros archivos de base de datos. App\_Data es un directorio especial de asp.net el cual mantiene la seguridad de los permisos de acceso a la base de datos. Desde el menú del proyecto, seleccione añadir folder ASP.NET, luego App_Data.  
–imagen  
Creando una cadena de conexión en el archivo web.config  
Añadiremos unas líneas al archivo de configuración así la entidad del marco de desarrollo conocerá como conectarse a nuestra base de datos. Doble clic en el archivo web.config localizado en la raíz del proyecto.  
–imagen  
Nos colocamos al final del documento y añadimos la sección directamente encima de la última línea como se muestra a continuación.

     
         
         
    
    

Añadiendo una clase de contexto  
Clic derecho sobre el folder de modelos y añadamos una nueva clase llamada MusicStoreEntities.cs  
–imagen  
Esta clase representara el contexto de la entidad del marco de desarrollo, y se encargara de las operaciones de crear, leer, actualizar y borrar por nosotros. El código de esta clase es como sigue.

    using System.Data.Entity;
    
    namespace MvcMusicStore.Models
    {
        public class MusicStoreEntities : DbContext
        {
            public DbSet&lt;Album&gt; Albums { get; set; }
            public DbSet&lt;Genre&gt; Genres { get; set; }
        }
    }
    

Eso es todo ninguna otra configuración, interfaces especiales, etc. Para extender la clase base DbContext, nuestra clase MusicStoreEntities está habilitada para manejar las operaciones de base de datos por nosotros. Ahora que lo colocamos, añadamos algunas propiedades más a nuestro modelo de clases para tomar ventaja de la información adicional en nuestra base de datos.  
Añadiendo nuestro catálogo de datos  
Vamos a tomar ventaja de la característica en la entidad del marco de desarrollo que añade datos “semilla” a una nueva base de datos. Esto pre-popula nuestro catálogo con una lista de géneros, artistas, y álbumes. Descargando el archivo MvcMusicStore-Assets.zip – el cual incluye los archivos del diseño del sitio usados en este tutorial – contiene un archivo clase con los datos semilla, localizado en el folder llamado Code.  
Dentro del folder Code/Models, localiza el archivo SampleData.cs y colócalo dentro del folder Modelos de nuestro proyecto como se muestra abajo.  
–imagen  
Ahora necesitamos añadir una línea de código que le dirá a la entidad del marco de desarrollo acerca de esta clase SampleData. Doble clic en el archivo Global.asax en la raíz del proyecto ábrelo y añade la siguiente línea al inicio del método Application_Start.

    protected void Application_Start()
    {
        System.Data.Entity.Database.SetInitializer(new MvcMusicStore.Models.SampleData());
        AreaRegistration.RegisterAllAreas();
        RegisterGlobalFilters(GlobalFilters.Filters);
        RegisterRoutes(RouteTable.Routes);
    }
    

Hasta este punto hemos completado el trabajo necesario para configurar la entidad del marco de desarrollo para nuestro proyecto.  
Consultas a la base de datos.  
Ahora debemos actualizar nuestro StoreController en lugar de usar nuestros “Datos de inicio” esto nos permitirá hacer llamadas a nuestra base de datos y hacer consultas de esta información. Iniciaremos declarando un campo en nuestro StoreController para mantener una instancia de nuestra clase MusicStoreEntities, llamada storeDB:

    public class StoreController : Controller
    {
        MusicStoreEntities storeDB = new MusicStoreEntities();
    

Actualizando el índice de la tienda a consultas de base de datos  
La clase MusicStoreEntities es mantenida por la entidad del marco de desarrollo y expone una colección de propiedades para cada tabla de nuestra base de datos. Actualicemos nuestro Acción Index en StoreController para recibir todos los géneros en nuestra base de datos.  
Anteriormente hicimos esto por medio de codificando nuestros datos. Ahora podemos instanciarlos directamente usando el contexto de la entidad de nuestro marco de desarrollo.  
Colección de Géneros:

    public ActionResult Index()
    {
        var genres = storeDB.Genres.ToList();
        return View(genres);
    }
    

No se necesitan hacer cambios en nuestra plantilla vista desde que estamos retornando el mismo StoreIndexViewModel nosotros lo retornamos anteriormente – ahora retornamos información directamente de nuestra base de datos.  
Cuando corremos nuestro proyecto de nuevo y visitamos la dirección “/Store”, veremos bien una lista de todos los géneros en nuestra base de datos:  
–imagen  
Actualizando la navegación y los detalles de la información corriente  
Con el método de acción /Store/Browse?genre=[some-genre] estamos buscando un género por nombre. Nosotros solamente esperamos un resultado, desde nunca hemos tenido dos entradas para el mismo nombre de género, y podemos usar la extensión .Single() en LINQ para consultar apropiadamente un objeto Género como esto (todavía no escribas esto):

    var example = storeDB.Genres.Single(g => g.Name == “Disco”);
    

El método simple toma la expresión lambda como un parámetro, que especifica que buscamos un único objeto género que igualara con el valor que hemos definido. En caso que haya más, estaremos cargando un único objeto género con el nombre de valor igualado a Disco.  
Aprovecharemos una característica de la entidad del marco de desarrollo que nos permite indicar otras entidades que buscamos cargar cuando el objeto género es recuperado. Esta característica es llamada Consulta de resultado modelado (Query Result Shaping), y nos habilita a reducir el número de veces que necesitamos para acceder a la base de datos para recuperar toda la información que necesitamos. Buscamos pre-recoger los álbumes por género que recibimos, así que actualizamos la consulta para incluirlo desde Genres.Include(“Albums”) para indicar que buscamos relacionarlo con álbumes. Esto es más eficiente ya que recibiremos ambos nuestros datos de género y álbum en una sola petición a la base de datos.  
Con la explicación expuesta, aquí esta como lucirá cuando se actualice el controlador Browse.

    public ActionResult Browse(string genre) 
    { 
        // Retrieve Genre and its Associated Albums from database 
        var genreModel = storeDB.Genres.Include("Albums") 
            .Single(g => g.Name == genre); 
        return View(genreModel); 
    }
    

Podemos actualizar la vista de la navegación para desplegar que álbumes están disponibles en cada género. Abramos la plantilla de la vista (la encontraras en /Views/Store/Browse.cshtml) y añadiremos una lista marcada de álbumes como se muestra a continuación.

    @model MvcMusicStore.Models.Genre 
    @{ 
        ViewBag.Title = "Browse"; 
    } 
    Browsing Genre: @Model.Name 
     
        @foreach (var album in Model.Albums) 
        { 
             
                @album.Title 
             
        } 
    
    

Corremos nuestra aplicación y navegamos a /Store/Browse?genero=Jazz que nos mostrara los resultados ahora siendo sacados de la base de datos, desplegando todos los álbumes de nuestro genero seleccionado.  
–imagen  
Ahora haremos el mismo cambio a nuestra dirección /Store/Details/[id] , y remplacemos nuestra información de prueba con una consulta a la base de datos que nos cargara un álbum cuyo ID es igual al valor del parámetro.

    public ActionResult Details(int id)
    {
        var album = storeDB.Albums.Find(id);
        return View(album);
    }

Corriendo nuestra aplicación y navegando a /Store/Details/1 nos mostrara los resultados traídos de la base de datos.  
–imagen  
Ahora para que nuestra página de detalles muestre un álbum por su álbum ID, debemos actualizar la vista de navegación a un link a la vista de detalles. Usaremos Html.ActionLink, exactamente como lo hicimos para enlazar nuestra página de inicio a nuestra navegación al final de la sección previa. Para el código completo de la vista de navegación aparece abajo.

    @model MvcMusicStore.Models.Genre 
    @{ 
        ViewBag.Title = "Browse"; 
    } 
    Browsing Genre: @Model.Name 
     
        @foreach (var album in Model.Albums) 
        { 
             
                @Html.ActionLink(album.Title, "Details", new { id = album.AlbumId }) 
             
        } 
    
    

Ahora somos capaces de navegar desde nuestra página de tienda a nuestra página de género, la cual lista los álbumes disponibles, y pulsando en un álbum podemos ver los detalles de cada álbum.