# Introducción: #  
Django es un framework de aplicaciones web  de código abierto para el lenguaje  Python, dada la tendencia actual Python empieza a ser uno de los lenguajes mas usados hoy en día, la tendencia va la alza por su facilidad de entendimiento y la comunidad del lenguaje.  

## Objetivo ##  
El objetivo de éste curso es aprender a montar una aplicación web con dejango y  las herramientas Leaflet y ChartJs.  
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
**python manage.py createapp app**  
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

Dado que estaremos trabajando con datos espaciales, tendremos que usar la extensión espacial de **django** la cual se puede consultar en el siguiente link **https://docs.djangoproject.com/en/3.0/ref/contrib/gis/**  
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
**Json** por sus siglas JavaScript Object Notation (notación de objetos javascritp) y siendo **Geojson** un tipo de json particular para datos espaciales, para más información consultar **https://www.json.org/json-es.html**  

## ¿Qué son los modelos django ##
Introduciremos el concepto de ORM (Object Relational Mapping) Objecto Modelo Relación en español, es un patrón de diseño que nos permite manejar las tablas de la base de datos como clases en el lenguaje, en nuestro caso Python, django internamente maneja un mapeo entre nuestras tablas en la base de datos y clases en python, es importante que la manupulación hacia las tablas de la base de datos es mejor hacerlo através del ORM que con ejecución de querys directamente a la base de datos por temas de seguridad.
<p align="center"> 
<img src="../img/Django-Models.png">
</p> 
### Cómo definir modelos en  python ###
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
Aunque resulte un poco obvio que IntegerField() y CharField() sean para campos de tipo entero y cadena respectivamente, no está de más leer o tener a la mano la documentación **https://docs.djangoproject.com/en/3.0/ref/contrib/gis/model-api/**, **https://docs.djangoproject.com/en/3.0/topics/db/models/**

