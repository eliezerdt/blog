---
title: 'Tutorial Music Store MVC 3 en español &#8211; Controladores'
author: admin
layout: post
permalink: /tutorial-music-store-mvc-3-en-espanol-controladores/
dsq_thread_id:
  - 
categories:
  - Artículos
---
# 

Esta entrada es parte y continuación del articulo [Tutorial Music Store MVC 3 en español][1]

 [1]: http://eliezerdiaz.com/tutorial-music-store-mvc-3-en-espanol/

![Controladores][2]

Controladores de apollo[saroy][3] /[Free Photos][4]

Usualmente cuando navegamos por una página web los links dentro de la aplicación nos dirigen a archivos tipo “/productos.aspx” o “/productos.php” y estos procesan la información para nosotros.

 [2]: http://eliezerdiaz.com/wp-content/uploads/2012/02/apollo-11-flight-controllers.jpg "Apollo 11 Flight Controllers"
 [3]: http://www.flickr.com/photos/49502979202@N01/
 [4]: http://foter.com/ "Free Photos"

En mvc las direcciones a la que accedemos nos dirigen a métodos o clases llamadas “controllers” y estos son los responsables de procesar nuestra petición.

OK, estamos hasta este punto, lo siguiente que haremos es crear nuestro controlador principal o la portada de nuestro sitio, por convención de mvc se llamara HomeController.

Click derecho sobre la carpeta controllers add-> controller

Le colocamos de nombre “HomeController” y esto nos creara un archivo con este contenido.



Comenzaremos reemplazando el contenido del método index para que retorne una cadena.

Cambiándolo por este código:



Ahora podemos ver cómo funciona nuestra aplicación pulsando el botón f5, esta acción nos abrirá una ventana en nuestro navegador y podremos ver una página con el texto que pusimos arriba.

Después de esto debemos de parar nuestra aplicación desde el menú build de nuestra aplicación donde dice stop debugging.

Ahora crearemos un nuevo controlador que manejara la parte de la tienda, este controlador tendrá 3 escenarios:

una página que listara los géneros de nuestra tienda

*   una página que navegara dentro de toda la lista de álbumes de un genero en particular
*   una página de detalle que mostrara información especifica de un álbum en particular.

Este nuevo control será creado de la misma manera que el anterior con el nombre “StoreController”

Este nuevo control ya es un método “index”. Usaremos este método para implementar la página que listara los géneros de nuestra tienda. Agregaremos dos métodos adicionales los otros dos escenarios que buscamos: Browse and Details.



Estos métodos (index,browse y Details) en nuestro control son llamadas “Acciones de control”, y como se abran dado cuenta en el control homecontroller.index, su trabajo es responder las peticiones que le entran desde la url y este determinan que contenido debe de enviarse de vuelta al navegador.

Si corres la aplicación ahorita puedes navegar dentro de este controlador con las siguientes direcciones:

*   /store
*   /store/browse
*   /store/details

Cada una de estas direcciones  devolverá las cadenas que se pusieron para cada una de los métodos.

Para continuar y dejar de devolver cadenas, le agregaremos un parámetro “genero” a nuestro método de acción, cuando MVC automáticamente pase un parámetro llamado genero nuestro método será invocado.



El método HttpUtility.HtmlEncode se asegura de que nadie ingrese código javascript en este parámetro.

Puedes correr tu aplicación y probar enviarle algún genero como parámetro /Store/Browse?Genre=Disco

Ahora modifiquemos la acción de detalles para leer y desplegar un parámetro llamado id. Ahora será diferente como manejaremos el valor del parámetro ya que no estará dentro de un querystring que es como lo hicimos anteriormente. Ahora será parte de la url.

Esto MVC lo hace de forma fácil, sin configurar nada. Por convención cuando MVC recibe la url y termina a rutiar los métodos de acción trata el último segmento después del método como un parámetro llamado “ID”. Si en la definición del método recibe un parámetro llamado id este pasará automáticamente.  
  
Probemos correr la aplicación y llamar a esta url: /Store/Details/5

En esta primera parte:

*   Creamos un nuevo proyecto en visual studio 10
*   Vimos la estructura de una aplicación MVC
*   aprendimos como correr un sitio usando los elementos que nos proporciona Visual studio.
*   Creamos dos clases de control: HomeController y StoreController
*   Agregamos métodos de acción a nuestros controladores que responden a peticiones Url y retornan texto a nuestro Browser