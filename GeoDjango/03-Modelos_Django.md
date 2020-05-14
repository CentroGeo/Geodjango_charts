## ¿Qué son los modelos django? ##
Introduciremos el concepto de ORM (Object Relational Mapping) Objecto Modelo Relación en español, es un patrón de diseño que nos permite manejar las tablas de la base de datos como clases en el lenguaje, en nuestro caso Python, django internamente maneja un mapeo entre nuestras tablas en la base de datos y clases en python, es importante que la manupulación hacia las tablas de la base de datos es mejor hacerlo através del ORM que con ejecución de querys directamente a la base de datos por temas de seguridad.
<p align="center"> 
<img src="../img/Django-Models.png">
</p> 

### Cómo definir modelos en  python ###
Existen dos formas de hacerlo un mapeo automático de tablas existentes en la base de datos o definir explicitamente nuestros modelos, para éste tema haremos ésto último. 


Debemos irnos al archivo **models.py** dentro de app y veremos lo siguiente:  
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
    id_ssc	= models.IntegerField()
    id_pgj = models.IntegerField()
    delito = models.CharField(max_length = 100)
    tipo_evento = models.CharField(max_length = 100)
    fecha = models.DateTimeField()	
    identidad = models.CharField(max_length = 100)	
```  
Como podremos observar nuestra variable id de tipo entero será nuestra llave en la tabla, posteriormente lo más importante a notar es la variable geom la cual funge como la representación de la geometría en nuestro caso es de tipo punto aunque podríamos tener otras como poligono o línea.   

Es **recomendable** más no obligatorio que se defina el modelo para cada app en su propio models.py
Aunque resulte un poco obvio que **IntegerField()** y **CharField()** sean para campos de tipo entero y cadena respectivamente como su nombre lo indica, no está de más leer o tener a la mano la documentación que se dejará en la parte de abajo.  

Dejaremos pendientes la parte de las relaciones entre tablas para más adelante, de momento no son necesarias.

1. [Documentación Django GIS Models][https://docs.djangoproject.com/en/3.0/ref/contrib/gis/model-api/]
2. [Documentación Django  Models][https://docs.djangoproject.com/en/3.0/topics/db/models/]
3. [Ejemplo Django GIS Models][https://youtu.be/jQlvPAiSCjk?t=994]
4. [Ejemplo Django  Models][https://www.youtube.com/watch?v=HDz6lqZ91rE]


