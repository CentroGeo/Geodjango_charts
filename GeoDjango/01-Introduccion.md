# Introducción: #  
Django es un framework de aplicaciones web  de código abierto para el lenguaje  Python, dada la tendencia actual Python empieza a ser uno de los lenguajes mas usados hoy en día, la tendencia va la alza por su facilidad de entendimiento y la comunidad del lenguaje.  

## Objetivo ##  
El objetivo de éste curso es aprender a montar una aplicación web básica con Django, junto con las herramientas Leaflet y ChartJs.

## Conocimientos previos ##
Se recomienda tener nociones intermedias de python, de lo contrario se recomienda apliamente revisar primero el curso de python básico e intermedio en el siguiente link.

Además, aunque se cubra un poco, se puede encontrar un tutorial básico de Javascript en el siguiente link:
## Instalación de Anaconda en Windows 10 ##  

<p align="center"> 
<img src="../img/Anaconda.png">
</p>  

**¿Qué es Anaconda?**   
Bueno pues Anaconda es una herramienta desarrollada por **Anaconda, Inc.** para Python y R que nos provee de bibliotecas, aplicaciones y herramientas muy útiles en el campo de la Ciencias de Datos sienod éste de código abierto.

Para descargar Anaconda en windows simplemente debemos ir al siguiente link  **https://www.anaconda.com/products/individual** donde se nos proporciona la edición individual libre de Anaconda. 

<p align="center"> 
<img src="../img/Anaconda01.png">
</p>  

Debemos dar en el botón de download y nos pedirá seleccionar la versión que queramos descargar, en nuestro caso bajaremos la versión de 64 bits con el instalador gráfico   
Abrimos el ejecutable .exe que nos descarga y veremos la siguiente pantalla: 

<p align="center"> 
<img src="../img/Anaconda02.png">
</p>  
<p align="center"> 
<img src="../img/Anaconda03.png">
</p>  
<p align="center"> 
<img src="../img/Anaconda04.png">
</p>  
<p align="center"> 
<img src="../img/Anaconda05.png">
</p> 
<p align="center"> 
<img src="../img/Anaconda06.png">
</p>

Indicamos la ruta de instalación, recomiendo dejarla tal cual la ponga si no estás familiarizado con las rutas.  

<p align="center"> 
<img src="../img/Anaconda07.png">
</p>  

Aquí debemos seleccionar las dos casillas, la primera lo que hace es agregar una variable de entorno para Anaconda en nuestro sistema operativo, ésto le indica al sistema operativo que independientemente de dónde estemos parados en la terminal podamos **"invocar"** a anaconda.  
Por otro lado, la otra casilla indica que le otorgamos que detecte el python de Anaconda por defecto al sistema operativo, tanto a herramientas como Visual Studio o similar, si no queremos que se use la versión de anaconda por defecto, entonces no debemos marcar esa casilla.  

<p align="center"> 
<img src="../img/Anaconda09.png">
</p>  
<p align="center"> 
<img src="../img/Anaconda10.png">
</p>  


Ahora tenemos instalado anaconda en nuestro sistema operativo.
## Creación de entorno ##

¿Qué es un entorno virtual? Es común que en situaciones de programación, en particular en python, conforme desarrollamos más y más programas nos enfrentemos a una situación donde tenemos tantas bibliotecas instaladas que entre ellas puedan causar algún tipo de conflicto, entonces dentro de lo que se conoce como **buenas prácticas de programación**, más en particular de python, existe algo llamado **entornos virtuales** que permiten separar las dependencias de cada proyecto hecho, es decir, no necesitamos tener instaladas todas las dependencias para un proyecto que tal vez no la requiera, la idea es tener solamente lo necesario para cada proyecto, entonces, en nuestro caso crearemos un entorno de **Anaconda** para nuestra versión de Python 3.7 que hasta éste punto del desarrollo del  tutorial es la versión por defecto que contiene. 

**Una vez abierta la terminal** debemos escribir el comando:  
<p align="center"> 
 
**conda create -n nombre_de_entorno python=3.7** 	 
</p> 

<p align="center"> 
<img src="../img/Anaconda11.png">
</p>  

Debemos escribir la opción **y** para crear el entorno y empezará a instalar las dependencias básicas del entorno.
## Activación del entorno ##  
Para activar nuestro entorno de django es necesario abrir una terminal ya sea **cmd** de Windows o algún emulador de terminal Linux como **cmder** **https://cmder.net/** cuya instalación en Windows se cubre en el curso básico de Python previamente mencionado.  

**conda activate nombre_de_entorno**  

Donde **nombre_de_entorno** corresponde al nombre con el que hayamos querido nombrar a nuestro entorno.
<h1>
[Curso](https://centrogeo.github.io/Geodjango_charts/GeoDjango/02-Creacion_proyecto.md)
</h1>  
