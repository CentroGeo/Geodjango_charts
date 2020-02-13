#Introducción:  
Django es un framework de aplicaciones web  de código abierto para el lenguaje  Python, dada la tendencia actual Python empieza a ser uno de los lenguajes mas usados hoy en día, la tendencia va la alza por su facilidad de entendimiento y la comunidad del lenguaje.  

##Objetivo  
El objetivo de éste curso es aprender a montar una aplicación web con dejango y  las herramientas Leaflet y ChartJs.  
##Activación del proyecto  
**conda activate entorno**  
##Creación de proyecto  
**django-admin startproject prueba**  
.  
![my image](../img/01.png)  
Se generará una carpeta con el nombres del proyecto y al abrirla encontraremos la siguiente estructura:  
.
|-- manage.py  
|  
|-- prueba  
.  
De ésta forma tendremos iniciado nuestro proyecto, ahora deberemos crear nuestra app de la siguiente forma:  
**python manage.py createapp app**  
Así habremos creado nuestra primera app, entremos a la carpeta y crearemos una carpeta dentro llamada vistaPrincipal y moveremos todos los archivos dentro de app a ella.  
La estructura debería quedar de la siguiente forma:  
.  
|
+-- prueba/                                  
|   
+-- manage.py      
+-- app/                              
|   +-- vistaPrincipal/  
|		+--archivos/  
|   +-- _init_.py  
  
  

