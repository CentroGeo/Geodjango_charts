# Introducción: #  
Django es un framework de aplicaciones web  de código abierto para el lenguaje  Python, dada la tendencia actual Python empieza a ser uno de los lenguajes mas usados hoy en día, la tendencia va la alza por su facilidad de entendimiento y la comunidad del lenguaje.  

## Objetivo ##  
El objetivo de éste curso es aprender a montar una aplicación web con Django y  las herramientas Leaflet y ChartJs.  
## Activación del proyecto ##  
**conda activate entorno**  
## Creación de proyecto ## 
**django-admin startproject prueba**  
.  
<p align="center"> 
<img src="../img/01.png">
</p>   
Se generará una carpeta con el nombres del proyecto y al abrirla encontraremos la siguiente estructura:  
.
```
Geodjango_charts/
└── prueba/  
	└── manage.py   
	└── prueba/  
```

De ésta forma tendremos iniciado nuestro proyecto, ahora deberemos crear nuestra app de la siguiente forma:  
**python manage.py startapp app**  
Así habremos creado nuestra primera app, entremos a la carpeta y crearemos una carpeta dentro llamada vistaPrincipal y moveremos todos los archivos dentro de app a ella.  
La estructura debería quedar de la siguiente forma:  
.
```
Geodjango_charts/
└── prueba/
   ├── manage.py
   ├── app/
       └── vistaPrincipal/
       		└── migrations/
       		└── admin.py
       		└── app.py
       		└── models.py
       		└── tests.py
       		└── views.py
       └── __init__.py
   ├── prueba/
       └── __init__.py
       └── __pycache__
       └── settings.py
       └── urls.py
       └── wsgi.py
```  
## Configuración del proyecto ##  
Hasta éste punto habremos creado nuestra app ahora debemos realizar unas pequeñas configuraciones en el archivo **settings.py** con el fin de tener la configuración lista para nuestra app.  
Abrimos el archivo settings.py y buscamos la sección de installed apps  
<p align="center"> 
<img src="../img/installed_apps_prev.png">
</p> 

Dado que estaremos trabajando con datos espaciales, tendremos que usar la extensión espacial de **django** la cual se puede consultar en el siguiente link ** https://docs.djangoproject.com/en/3.0/ref/contrib/gis/ **  
<p align="center"> 
<img src="../img/installed_app_post.png">
</p>  
Con ésto estamos usando la extensión espacial y de paso incorporando la herramienta de leaflet para django, leaflet es una herramienta de **Javascript** para la creación de mapas interactivos, se puede encontrar su documentación en el siguiente link **https://leafletjs.com/reference-1.6.0.html**.
**Nota** Se pueden agregar tantas apps queramos crear, solo debemos agregarla dentro de installed apps como le hicimos con la de **app**.
Una vez hecho lo anterior procedemos a indicar la ubicación de la carpeta donde estará nuestros templates de html por lo que la parte de templates deberá quedar de la siguiente forma:  

<p align="center"> 
<img src="../img/prev_templates.png">
</p>  
<p align="center"> 
<img src="../img/post_templates.png">
</p> 
Como paso previo deberemos haber creado una base de datos en posgtres y crear la extensión espacial de postgis en ella, no importa que aun no tenga tablas.  
Procedemos a configurar la base de datos dado que por defecto django usa sqlite3 por lo que la configuración debe quedar de la siguiente forma:  

<p align="center"> 
<img src="../img/prev_db.png">
</p>
<p align="center"> 
<img src="../img/post_db.png">
</p> 
A nuestra base de datos la llamaremos accidentes, pueden usar el nombre que quieran pero debe coincidir con el nombre de la base que crearon al igual que la contraseña.  
Como requisito debemos instalar psycopg2 o tenerlo instalado, éste es el controlador para realizar la conexión a Postgres, recordemos que todo debe instalarse en el entorno de conda y através de **conda install package**.  
Ahora solo nos queda configurar para poder cargar archivos estáticos, es decir archivos js, img, css que vamos  a requerir más adelante.  

<p align="center"> 
<img src="../img/static_conf.png">
</p> 

Con ésto tenemos la configuración básica de nuestra aplicación hecha, aunque aún podemos modificarle cosas se dejará para más adelante.

Antes de continuar es importante mencionar el concepto de **json y geojson**  
**Json** por sus siglas JavaScript Object Notation (notación de objetos javascritp) y siendo ** Geojson ** un tipo de json particular para datos espaciales, para más información consultar **https://www.json.org/json-es.html**  

## ¿Qué son los modelos django ##
Introduciremos el concepto de ORM (Object Relational Mapping) Objecto Modelo Relación en español, es un patrón de diseño que nos permite manejar las tablas de la base de datos como clases en el lenguaje, en nuestro caso Python, django internamente maneja un mapeo entre nuestras tablas en la base de datos y clases en python, es importante que la manupulación hacia las tablas de la base de datos es mejor hacerlo através del ORM que con ejecución de querys directamente a la base de datos por temas de seguridad.
<p align="center"> 
<img src="../img/Django-Models.png">
</p> 
###Cómo definir modelos en  python###
Debemos irnos al archivo models.py dentro de app y veremos lo siguiente:  
<p align="center"> 
<img src="../img/prev_models.png">
</p>   
Aquí debemos definir nuestros mapeos de la base de datos como se mencionó anteriormente, para crear un modelo que tenga soporte para datos espaciales debemos primero hacer el siguiente **import** hasta arriba de nuestro archivo

```python
from django.contrib.gis.db import models as geomodels
```  
con ello estaremos diciéndole a django que use la extensión espacial y podemos definir nuestros modelos de la siguiente forma, haremos una clase ejemplo  

```python
from django.db import models
from django.contrib.gis.db import models as geomodels

# Create your models here.

class Datos(models.Model):
    id = models.IntegerField(primary_key=True)    	
    geom = geomodels.MultiPointField()
    field_1	= models.IntegerField()
    id_ssc	= models.IntegerField()
    id_pgj = models.IntegerField()
    delito = models.CharField(max_length = 100)
    tipo_evento = models.CharField(max_length = 100)
    fecha = models.DateTimeField()	
    identidad = models.CharField(max_length = 100)	
```  
Como podremos observar nuestra variable id de tipo entero será nuestra llave en la tabla, posteriormente lo más importante a notar es la variable geom la cual funge como la representación de la geometría en nuestro caso es de tipo punto aunque podríamos tener otras como poligono o línea. Es **recomendable** más no obligatorio que se defina el modelo para cada app en su propio models.py
Aunque resulte un poco obvio que IntegerField() y CharField() sean para campos de tipo entero y cadena respectivamente, no está de más leer o tener a la mano la documentación **https://docs.djangoproject.com/en/3.0/ref/contrib/gis/model-api/**, **https://docs.djangoproject.com/en/3.0/topics/db/models/**.  
Dejaremos pendientes la parte de las relaciones entre tablas para más adelante, de momento no son necesarias..

## Rutas **URL** y vistas ##  
Para ésto es importante mencionar qué es HTTP (Protocolo de Transferencia de Hipertexto), es un protocolo de transferencia de información hipermedia para Internet, diseñado para la comunicación entre navegadores y servidores web, como lo que estaremos montando es un servidor web.  
Nos iremos al archivo **views.py** dentro de app por lo que tendremos el siguiente código:  
```python
from django.shortcuts import render

# Create your views here.
```  
Importaremos las siguientes bibliotecas:  
```python
from django.shortcuts import render
import json
from geojson import Point, Feature, FeatureCollection, dump
import pandas as pd
import geopandas

from django.contrib.gis.geos import Polygon, Point, MultiPoint, GeometryCollection
from shapely.geometry import Point, mapping, shape
from .models import * 
from django.views.decorators.csrf import csrf_exempt

from django.http import *

from django.core.serializers import serialize
from django.core  import *
#Create your views here
```  
Entonces definiremos nuestra ruta para el index, el cual siempre funge como la página principal para ésto mencionaremos brevemente los dos métodos **HTTP** más comunes:  
*   GET El método GET  solicita una representación de un recurso específico. Las peticiones que usan el método GET sólo deben recuperar datos.    
*   POST  El método POST se utiliza para enviar una entidad a un recurso en específico, causando a menudo un cambio en el estado o efectos secundarios en el servidor.    

Procedemos a definir el index (home) de la siguiente manera:
```python
@csrf_exempt

def index(request):
    if request.method == 'GET':
        return render(request,'primeraVista/home.html')
    elif request.method == 'POST':
        
        return HttpResponseForbidden()
```  
De momento no necesitamos definir un comportamiento para el método POST hacia nuestro index por lo que  usamos 403 de http para denegar la petición, solo nos interesa mandar peticiones de tipo GET al servidor para renderizar el html de la página principal, aunque la convención es a la primera vista llamarla **index.html**, posteriormente debemos ir a nuestro archivo **urls.py** dentro de la carpeta prueba y deberá verse de la siguiente forma:  
<p align="center"> 
<img src="../img/prev_urls.png">
</p>  

Como podremos observar solo tenemos la ruta para el admin propio de django, ahora en éste arreglo debemos definir la ruta para cada ruta que tengamos en nuestro servidor django, de 
momento solo tendremos la que contendrá el mapa en leaflet, por lo que deberemos tener el siguiente código en nuestro archivo **prueba/urls.py**:

```python
from django.contrib import admin
from django.urls import path
from django.conf.urls import url ,include
from app.vistaPrincipal.urls import *
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', index),
]
```
Y crearemos un nuevo archivo **urls.py** dentro de la carpeta vistaPrincipal con el siguiente contenido:

```python
from django.urls import path
from django.conf.urls import url,include
from app.vistaPrincipal.views import *
urlpatterns = [
    path('^$',index),
]
```  
Entre las comillas debemos escribir la ruta que deberá tomar, cuando dejamos las comillas en vacío le estamos indicando que la ruta por defecto es "http://127.0.0.1:8000".  

Previo a iniciar por primera vez nuestro servidor debemos aplicar las **migrations** las cuales son la forma en la que django aplica los cambios a los modelos definidos y con el comando **makemigrations** se crean nuevas migraciones sobre la base de datos, es por ello que deberemos ejecutar los siguientes comandos:  
**python manage.py migrate**
<p align="center"> 
<img src="../img/prev-migrate.png">
</p>  

**python** **manage.py** **makemigrations**    

<p align="center"> 
<img src="../img/prev-makemigrations.png">
</p> 

Ahora si podremos inicar nuestra aplicación para comprobar si todo va correctamente, ejecutaremos el comando **python** **manage.py** **runserver** y si vamos a nuestro navegador con la ruta "http://127.0.0.1:8000" obtendremos lo siguiente:  
<p align="center"> 
<img src="../img/prev_django.png">
</p>  



## Creación de nuestro archivo home.html
Debemos crear las carpetas static y templates por convención se deben usar esos nombres, con lo cual nuestro proyecto debe tener la siguiente estructura:  
```
Geodjango_charts/
└── prueba/
   ├── manage.py
   ├── app/
       └── vistaPrincipal/
       		└── migrations/
       		└── admin.py
       		└── app.py
       		└── models.py
       		└── tests.py
       		└── views.py
       		└── urls.py
       └── __init__.py
   ├── prueba/
       └── __init__.py
       └── __pycache__
       └── settings.py
       └── urls.py
       └── wsgi.py
    ├── templates/
    	└── primeraVista
    		└──home.html
    ├── static/
    	└── img/
    	└── js/
    	└── css/
```
agregar lo de app.vistaPrincipal.url  
Ahora dentro de la carpeta templates/primeraVista creamos un archivo **home.html** con la siguiente estructura básica de html5  

<p align="center"> 
<img src="../img/prev_home_html.png">
</p>  

En donde **head** es donde haremos todos los imports de las bibliotecas externas y archivos css.
Con lo que ahora podremos visualizar lo siguiente en nuestro navegador:  

<p align="center"> 
<img src="../img/ejemplo-html.png">
</p>

Debemos escribir hasta arriba de nuestro html lo siguiente:  

<p align="center"> 
<img src="../img/static-tags.png">
</p> 

**Hasta éste punto ya tenemos nuestro template listo para montar nuestro mapa** por lo que procedemos a importar leaflet, pero antes haremos el import de las cosas necesarias externas y otros tags para leaflet:  

<p align="center"> 
<img src="../img/leaflet-tags.png">
</p>  

```html
    <link href="https://fonts.googleapis.com/css?family=PT+Sans&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css?family=Bebas+Neue&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css?family=Encode+Sans+Semi+Condensed|IBM+Plex+Mono|PT+Sans|Titillium+Web&display=swap" rel="stylesheet">
      
    <!--JQuery-->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
    
    <!--Fonts necesarios-->
    <link href="https://fonts.googleapis.com/css?family=PT+Sans&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css?family=Bebas+Neue&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css?family=Indie+Flower|Open+Sans|Open+Sans+Condensed:300|Raleway|Roboto+Mono|Roboto+Slab&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css?family=Encode+Sans+Semi+Condensed|IBM+Plex+Mono|PT+Sans|Titillium+Web&display=swap" rel="stylesheet">
    <!-- Scripts necesarios para chart js-->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.2/Chart.bundle.js" integrity="sha256-JG6hsuMjFnQ2spWq0UiaDRJBaarzhFbUxiUTxQDA9Lk=" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.2/Chart.bundle.min.js" integrity="sha256-XF29CBwU1MWLaGEnsELogU6Y6rcc5nCkhhx89nFMIDQ=" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.2/Chart.js" integrity="sha256-J2sc79NPV/osLcIpzL3K8uJyAD7T5gaEFKlLDM18oxY=" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.2/Chart.min.js" integrity="sha256-CfcERD4Ov4+lKbWbYqXD6aFM9M51gN4GUEtDhkWABMo=" crossorigin="anonymous"></script>
    
```  

Ahora necesitamos agregar nuestros propios archivos así que haremos los siguientes imports:  

<p align="center"> 
<img src="../img/staticfiles.png">
</p>  

Crearemos un div general dentro del cual estárá todo el contenido (se recomienda leer un poco acerca del DOM de los navegadores para su mejor entendimiento), el cual tendrá una clase grid-container para colocar de mejor forma los elementos dentro del html.
Los eleméntos básicos para el curso dentro del body quedan así, explicaremos cada uno de ellos:  
```html  
<div class="grid-container">

      <div class='select'>
            <select name="combo-box" id="combo-box">
              <option value="2018">AÑO 2018</option>
              <option value="Enero">Enero</option>
              <option value="Febrero">Febrero</option>
              <option value="Marzo">Marzo</option>
              <option value="Abril">Abril</option>
              <option value="Mayo">Mayo</option>
              <option value="Junio">Junio</option>
              <option value="Junio">Julio</option>
              <option value="Agosto">Agosto</option>
              <option value="Septiembre">Septiembre</option>
              <option value="Octubre">Octubre</option>
              <option value="Noviembre">Noviembre</option>
              <option value="Diciembre">Diciembre</option>
            </select> 
          </div>
          
          <div class ='button-div'> 
            <button class="button" type="button" onclick="mapaCalor()" >ACEPTAR</button>
          </div>
        
        <div class="item3" id="map1" style="min-zoom: 3"></div>    
      
      </div>

      
    <div class="chart-container" >
      <canvas id="myChartGraph" width="100" height="100" style="margin-left:100px;"></canvas> 
      <canvas id="myChartGraph1" width="100" height="100" style="margin-left:200px;"></canvas> 
    </div>


```  

El tag **select** nos sirve para desplegar un **combo-box** al cual através de **id** le asignamos un identificador para trabajar con él más adelante y lo llenamos con los valores que en nuestro caso nos conviene alberguen los meses del año y opciones adicionales y através del tag **option** podemos meter las opciones al combo-box, **value** le asigna un valor de tipo cadena el cual es el que lograremos obtener desde JavaScript para saber cuál fue la selección del usuario.  
En el tag **button** le  estamos indicando que cuando sea apretado ejecute una llamada a función en nuestro caso le llamaremos mapaCalor(), así también le pondremos el mensaje de Aceptar para que el usuario después de seleccionar el mes que desea pueda filtrar los datos.  
Debemos crear un div exclusivo para nuestro mapa de leaflet, le asignaremos el **id="map1"** y la clase de css **class="item3"**, se podría decir que solo estamos definiendo el contenedor para el mapa de momento y dejaremos pendiente las gráficas para más adelante en el curso.  

Creamos un nuevo archivo llamado mapa.css y lo guardaremos dentro de la carpeta **static/css/** para más adelante poder acomodar nuestros elementos, así también copiaremos los archivos heatmat.js y leaflet-heatmap.js dentro de nuestra carpeta **static/js/** y también crearemos dentro de dicha carpeta un archivo **datos.js** con lo que nuestra estructura actualizada quedaría de la siguiente manera:  

```
Geodjango_charts/
└── prueba/
   ├── manage.py
   ├── app/
       └── vistaPrincipal/
          └── migrations/
          └── admin.py
          └── app.py
          └── models.py
          └── tests.py
          └── views.py
          └── urls.py
       └── __init__.py
   ├── prueba/
       └── __init__.py
       └── __pycache__
       └── settings.py
       └── urls.py
       └── wsgi.py
    ├── templates/
      └── primeraVista
        └──home.html
    ├── static/
      └── img/
      └── js/
        └──datos.js
        └──heatmap.js
        └──leaflet-heatmap.js
      └── css/
        └──mapa.css
```  

# Referencias
1.  Mozilla, Mozilla org, Lunes 17 Febrero 2019, HTTP, https://developer.mozilla.org/es/docs/Web/HTTP. 
