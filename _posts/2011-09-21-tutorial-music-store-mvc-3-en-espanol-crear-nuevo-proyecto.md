---
title: 'Tutorial Music Store MVC 3 en español &#8211; Crear nuevo proyecto'
author: admin
layout: post
permalink: /tutorial-music-store-mvc-3-en-espanol-crear-nuevo-proyecto/
dsq_thread_id:
  - 
categories:
  - Artículos
---
# 

Esta entrada es parte y continuación del articulo [Tutorial Music Store MVC 3 en español][1]

 [1]: http://eliezerdiaz.com/tutorial-music-store-mvc-3-en-espanol/

 

Creamos un nuevo proyecto web en visual c#

usamos un template vacio

Utilizamos razor como motor de plantillas y marcamos que vamos a utilizar html5

Luego de eso veremos que se nos crearan varias carpetas dentro del proyecto vacio

![estructura de archivos mvc][2]
estructura de archivos mvc

Estas son:

 [2]: http://www.eliezerdiaz.com/wp-content/uploads/2011/09/015.jpg "estructura de archivos mvc"

/controllers : los archivos aquí contenidos responden a las llamadas de navegador y deciden que es lo que se responderá al usuario

/views: en esta carpeta están las vistas de la aplicación

/modelos: estos archivos manejan los datos de la aplicación

/este folder contiene los archivos css, imágenes y cualquier otro recurso estático

/scripts: en este folder están los archivos javascripts de los archivos

Por defecto los archivos que se encuentran en la carpeta controllers tienen acceso a los archivos que están guardados en la carpeta de vistas sin necesidad de escribirlo explícitamente en el código.