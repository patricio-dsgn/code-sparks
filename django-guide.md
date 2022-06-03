# Guia - Django

## crear entorno virtual

```console
pip3 install pipenv       # instala pipenv
pipenv shell              # crea o activa un entorno virtual con pipenv
exit                      # salir de virtual pipenv

            pipenv --rm   # borra entorno virtual de pipenv
```

## instalar dependencias

```console
pip freeze > requirements.txt        # crea archivo para instalar las dependencias

                                     # instalar dependencias

pip install django                   # instala django u otras dependencias
pip install Pillow                   # instala Pillow para manejar imágenes
            pip install django --dev # instala en desarrollo
            pip uninstall django     # desinstala django

            pip install mysqlclient  # para usar MySQL
            pip install psycopg2     # para usar PostgreSQL

                                    # instalar en bloque

            pipenv install -r requirements.txt   # instala desde requirements.txt
            pipenv install                       # instala desde Pipfile
            pipenv install --ignore-pipfile      # instala desde Pipfile.lock

                            # extras
            pipenv freeze   # muestra lo que ha instalado pipenv
            pipenv lock -r  # muestra lo que necesita pipenv
            pipenv lock     # actualiza Pipfile.lock para despliegue
            pipenv graph    # muestra lo que necesita pipenv
            pipenv check    # muestra las vunerabilidades, si debes actualizar
```

## crear proyecto y app

```console
django-admin startproject demo .       # crea proyecto (con un punto al final)
python manage.py startapp website      # crea app
```

```bash

root                         # directorio raiz
|
|
|──manage.py                 # utilidad para manejar Django
|──requirements.txt          # archivo con lista de modulos necesarios
|──db.sqlite3                # base de datos
|
|──README.md                 # documentación
|──.gitignore                # archivos a ignorar por GIT
|
|──Pipfile                   # info sobre entirno virtual
|──Pipfile.lock              # info sobre entirno virtual
|
|──db.json                   # json con datos (de base de datos)
|
|──media                     # directorio para subir imagenes
|──uploads                   # directorio para subir documentos
|
|──demo                      # directorio proyecto
|    |
|    |──__init__.py   # indica que este directorio sea leído
|    |──settings.py   # configuraciones generales
|    |──urls.py       # rutas de las apps
|    |──asgi.py       # despliege asincrono
|    └──wsgi.py       # conección con servidor(punto de entrada)
|
|
|──website                 # directorio aplicación
|    |
|    |──migrations         # directorio migraciones (cambios en estructura base datos)
|    |    └──__init__.py
|    |
|    |──static             # directorio de archivos estáticos
|    |    └──website       # debe seguir la estructura: website/static/website/...
|    |         |──css
|    |         |   └──style.css
|    |         |
|    |         |──img
|    |         |   |──logo.svg
|    |         |   |──background.jpg
|    |         |   └──profile.png
|    |         |
|    |         └──js
|    |             └──script.js
|    |
|    |──templates           # directorio plantillas
|    |    └──website        # debe seguir la estructura: website/templates/website/...
|    |         |──base.html # archivo base
|    |         |──home.html
|    |         |──contact.html
|    |         └──about.html
|    |
|    └──templatestags       # directorio templates personalizador y filtros
|         |──__init__.py
|         └──myapp_tags.py
|    
|
|──NAME_APP2    # directorio aplicación 2...
|
|──NAME_APP3    # directorio aplicación 3...
|
└──NAME_APP4    # directorio aplicación 4...
```

## Integrar app, extends, templates, statics, y materialize css

**demo/settings.py**

```python
import os # templates

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
   'website',# agregar app
]
```

```python
TEMPLATES = [
 {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR,'templates')], # templates - agregar ruta
       'APP_DIRS': True,
       'OPTIONS': {
         'context_processors': [
            'django.template.context_processors.debug',
            'django.template.context_processors.request',
            'django.contrib.auth.context_processors.auth',
            'django.contrib.messages.context_processors.messages',
        ],
  },
 },
]
```

```python
LANGUAGE_CODE = 'es-es'

TIME_ZONE = 'America/Santiago'
```

**demo/urls.py**

```python
from django.contrib import admin
from django.urls import path, include # templates - agregar include 
from django.conf import settings # static
from django.conf.urls.static import static # static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('website.urls')), # templates - agregar ruta app
] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT) # static

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

**website/views.py**

```python
from django.http import HttpResponse
from django.shortcuts import render, redirect
from website.models import *
# from website.models import Person
  
def hello(request):
    return HttpResponse('Hello, World!')

def home(request):
    return render(request,'website/home.html',{'sel':'home'})

def about(request):
    query_results = Person.objects.all()
    return render(request,'website/about.html',{'query_results':query_results, 'sel':'about'})

def contact(request):
    return render(request,'website/contact.html',{'sel':'contact'})
```

**website/urls.py**

```python
from django.urls import path
from . import views
#from .views import home, about, contact, hello

urlpatterns = [
  path('', views.home, name='home'),
  path('about', views.about, name='about'),
  path('contact', views.contact, name='contact'),
  path('hello', views.hello, name='hello'),
  path('../admin/', views.hello, name='admin'),
]
```

**website/models.py**

```python
from django.db import models

class Person(models.Model):
    ICONS = (
        ('fa-solid fa-droplet', 'agua'),
        ('fa-solid fa-wheat-awn', 'comida'),
        ('fa-solid fa-comment-medical', 'medicamento'),
    )
    title = models.CharField(max_length=100)
    text = models.TextField()
    tag = models.CharField(max_length=100, blank=True)
    date = models.DateTimeField(auto_now_add=True)
    photo = models.ImageField(
        upload_to='media/person',
        default='media/person/default.jpg',
        blank=True)
    upload = models.FileField(upload_to='uploads/doc')
    url = models.URLField(blank=True)
    number_integer = models.IntegerField(default=0)
    number_decimal = models.DecimalField(
        max_digits=10, decimal_places=2, default=0)
    number_float = models.FloatField(default=0)
    icon = models.CharField(max_length=100, choices=ICONS)

    class Meta:
        # verbose_name = 'Person'
        verbose_name_plural = 'Personas'
```

**website/admin.py**

```python
from django.contrib import admin
from . models import *
# from . models import Person

class table_format1(admin.ModelAdmin):
    list_display = ('title','date','number_integer')

admin.site.register(Person, table_format1)
```

**en directorio**

```bash
root
|
|──media
|
└──uploads
```

```bash
root
|
└──website
    └──templates
        └──website
            |──base.html
            |──home.html
            |──about.html
            └──contact.html
```

```bash
root
|
└──website
    └──templatetags
        |──__init__.py
        └──myapp_tags.py
```

```bash
website
|
└──static
    └──website
        |──css
        |   └──base.css
        |
        |──img
        |   |──logo.svg
        |   |──background.jpg
        |   └──profile.png
        |
        └──js
            └──script.js                                      
```

**en myapp_tags.py**

```python
from django import template

register = template.Library()

@register.filter(name='split')
def split(str, key):
    return str.split(key)    
```

## Template Materialize CSS

**html - base.html**

```django
{% load static %}
<!DOCTYPE html>
<html lang="en">

<head>
    <title>{% block title %} Hello World {% endblock title %}</title>
    <link rel="stylesheet" href="{% static '/website/css/style.css' %}">
    <!-- Compiled and minified CSS -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/css/materialize.min.css">
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">

</head>

<body>
    <nav>
        <div class="nav-wrapper">
            <a href="{% url 'home' %}" class="brand-logo">
                <img class="logo" src="{% static '/website/img/logo.svg' %}" alt="logo">
            </a>
            <a href="#" data-target="mobile-demo" class="sidenav-trigger"><i class="material-icons">menu</i></a>
            <ul class="right hide-on-med-and-down">
                <li {% if sel == 'home' %} class="active" {% endif %}><a href="{% url 'home' %}">home</a></li>
                <li {% if sel == 'about' %} class="active" {% endif %}><a href="{% url 'about' %}">about</a></li>
                <li {% if sel == 'contact' %} class="active" {% endif %}><a href="{% url 'contact' %}">contact</a></li>
                <li><a href="{% url 'hello' %}">hello</a></li>
            </ul>
        </div>
    </nav>

    <ul class="sidenav" id="mobile-demo">
        <img class="logo" src="{% static '/website/img/logo.svg' %}" alt="logo">
        <li {% if sel == 'home' %} class="active" {% endif %}><a href="{% url 'home' %}">home</a></li>
        <li {% if sel == 'about' %} class="active" {% endif %}><a href="{% url 'about' %}">about</a></li>
        <li {% if sel == 'contact' %} class="active" {% endif %}><a href="{% url 'contact' %}">contact</a></li>
        <li><a href="{% url 'hello' %}">hello</a></li>
    </ul>

    {% block content %}contenido{% endblock content %}



    <footer class="page-footer">
        <div class="container">
            <div class="row">
                <div class="col l6 s12">
                    <h5 class="white-text">Footer Content</h5>
                    <p class="grey-text text-lighten-4">You can use rows and columns here to organize your footer
                        content.</p>
                </div>
                <div class="col l4 offset-l2 s12">
                    <h5 class="white-text">Links</h5>
                    <ul>
                        <li><a class="grey-text text-lighten-3" href="#!">Link 1</a></li>
                        <li><a class="grey-text text-lighten-3" href="#!">Link 2</a></li>
                        <li><a class="grey-text text-lighten-3" href="#!">Link 3</a></li>
                        <li><a class="grey-text text-lighten-3" href="#!">Link 4</a></li>
                    </ul>
                </div>
            </div>
        </div>
        <div class="footer-copyright">
            <div class="container">
                © 2014 Copyright Text
                <a class="grey-text text-lighten-4 right" href="{% url 'admin' %}">admin</a>
            </div>
        </div>
    </footer>


    <script src="{% static '/website/js/script.js' %}"></script>
    <!-- Compiled and minified JavaScript -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js"></script>

    <script>
        document.addEventListener('DOMContentLoaded', function () {



            var elems = document.querySelectorAll('.sidenav');
            var instances = M.Sidenav.init(elems);

            var elems = document.querySelectorAll('.materialboxed');
            var instances = M.Materialbox.init(elems);

            var elems = document.querySelectorAll('.datepicker');
            var instances = M.Datepicker.init(elems);

        });
    </script>
</body>

</html>
````

**html - home.html**

```django
{% extends 'website/base.html' %}
{% load static %}
{% block title %}Home{% endblock title %}
{% block content %}
<div class="container section">

    <h1>home</h1>
    <div class="row red section">

        <div class="col s12 m6 ">
            <img class="responsive-img" src="{% static '/website/img/profile.png' %}">
        </div>
        <div class="col s12 m6 ">
            <h4>Lorem ipsum dolor sit amet</h4>
            <div class="divider"></div>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.
                Ipsa corrupti, ad aspernatur porro qui in aperiam,
                dolor dolore quis, debitis animi eligendi velit
                assumenda! Porro rerum molestias assumenda aliquid sed.</p>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing
                elit. Ipsa corrupti, ad aspernatur porro qui in aperiam</p>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing
                elit. Ipsa corrupti</p>
        </div>

    </div>

</div>

{% endblock %}
```

**html - about.html**

```django
{% extends 'website/base.html' %}
{% load static %}
{% block title %}about{% endblock title %}
{% block content %}
<div class="container section">

    <h1>about</h1>
    <div class="row cyan section">

        <div class="col s12 m6 ">
            <table class="responsive-table">
                <thead>
                  <tr>
                      <th>Name</th>
                      <th>Item Name</th>
                      <th>Item Price</th>
                  </tr>
                </thead>
        
                <tbody>
                  <tr>
                    <td>Alvin</td>
                    <td>Eclair</td>
                    <td>$0.87</td>
                  </tr>
                  <tr>
                    <td>Alan</td>
                    <td>Jellybean</td>
                    <td>$3.76</td>
                  </tr>
                  <tr>
                    <td>Jonathan</td>
                    <td>Lollipop</td>
                    <td>$7.00</td>
                  </tr>
                </tbody>
              </table>
                    
        </div>
        <div class="col s12 m6 ">
            <h4>Lorem ipsum dolor sit amet</h4>
            <div class="divider"></div>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.
              Ipsa corrupti, ad aspernatur porro qui in aperiam,
              dolor dolore quis, debitis animi eligendi velit
              assumenda! Porro rerum molestias assumenda aliquid sed.</p>
            <div class="card-panel cyan darken-4 white-text">This is a card panel with a teal lighten-2 class</div>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing
                elit. Ipsa corrupti, ad aspernatur porro qui in aperiam</p>
            <blockquote>
              This is an example quotation that uses the blockquote tag.
            </blockquote>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing
                elit. Ipsa corrupti</p>
            <p class="flow-text">I am Flow Text</p>
        </div>

    </div>

</div>

{% endblock %}
```

**html - contact.html**

```django
{% extends 'website/base.html' %}
{% load static %}
{% block title %}contact{% endblock title %}
{% block content %}

<div class="container section">

    <h1>contact</h1>
    <div class="row green lighten-4 section">

        
        <form class="col m8">
            <div class="row">
                <div class="input-field col s6">
                    <input placeholder="Placeholder" id="first_name" type="text" class="validate">
                    <label for="first_name">First Name</label>
                </div>
                <div class="input-field col s6">
                    <input id="last_name" type="text" class="validate">
                    <label for="last_name">Last Name</label>
                </div>
            </div>
            <div class="row">
                <div class="input-field col s12">
                    <input disabled value="I am not editable" id="disabled" type="text" class="validate">
                    <label for="disabled">Disabled</label>
                </div>
            </div>
            <div class="row">
                <div class="input-field col s12">
                    <input id="password" type="password" class="validate">
                    <label for="password">Password</label>
                </div>
            </div>
            <div class="row">
                <div class="input-field col s12">
                    <input id="email" type="email" class="validate">
                    <label for="email">Email</label>
                </div>
            </div>
            <div class="row">
                <div class="col s12">
                    This is an inline input field:
                    <div class="input-field inline">
                        <input id="email_inline" type="email" class="validate">
                        <label for="email_inline">Email</label>
                        <span class="helper-text" data-error="wrong" data-success="right">Helper text</span>
                    </div>
                </div>
            </div>
            <p>
                <label>
                  <input type="checkbox" />
                  <span>Red</span>
                </label>
              </p>
              <p>
                <label>
                  <input type="checkbox" checked="checked" />
                  <span>Yellow</span>
                </label>
              </p>
              <p>
                <label>
                  <input type="checkbox" class="filled-in" checked="checked" />
                  <span>Filled in</span>
                </label>
              </p>
              <p>
                <label>
                  <input id="indeterminate-checkbox" type="checkbox" />
                  <span>Indeterminate Style</span>
                </label>
              </p>
              <p>
                <label>
                  <input type="checkbox" checked="checked" disabled="disabled" />
                  <span>Green</span>
                </label>
              </p>
              <p>
                <label>
                  <input type="checkbox" disabled="disabled" />
                  <span>Brown</span>
                </label>
              </p>
              <input type="text" class="datepicker">

        </form>
    </div>

</div>

{% endblock %}
```


## migraciones y admin

```console
python manage.py makemigrations   # empaqueta las migraciones(si hay cambios en models.py)
python manage.py migrate          # aplica las migraciones
python manage.py createsuperuser  # crea superusuario (seguir instrucciones de consola)
````

ingresar en browser [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)

```console
python manage.py runserver        # levanta servidor de django
python manage.py runserver 8000   # levanta servidor de django
python manage.py runserver 8001   # levanta servidor de django
```


## transferir base de datos

```console
python manage.py dumpdata > db.json # guarda data en json
```
**cambiar configuracion en settings.py**

_MYSQL_
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql'
        'NAME': 'name_db'
        'USER': 'name_user'
        'PASSWORD': 'pass'
        'HOST': 'localhost',   # Or an IP Address that your DB is hosted on
        'PORT': '3306'
    }
}
```

_POSTGRESS_
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2'
        'NAME': 'name_db'
        'USER': 'name_user'
        'PASSWORD': 'pass'
        'HOST': 'localhost',   # Or an IP Address that your DB is hosted on
        'PORT': '5432'
    }
}
```

```console
python manage.py migrate           # migrar base de datos
```

```console
python manage.py shell             # fomatear base
```

```console
from django.contrib.contenttypes.models import ContentType
ContentType.objects.all().delete()
```

```console
python manage.py loaddata db.json # importar datos json
```


---

## deploy Django en Heroku (11 pasos)

# 1. Instalar 'HEROKU CLI'
<https://devcenter.heroku.com/articles/heroku-cli#install-the-heroku-cli>

# 2. En 'Consola'

python manage.py dumpdata > db.json
python manage.py loaddata db.json

    # gestionar las base de datos
    pip install gunicorn

    # gestionar las base de datos
    pip install dj-database-url

    # gestionar las variables de entornos
    pip install python-decouple

    # gestionar los archivos estáticos      
    pip install whitenoise

#

pip freeze > requirements.txt

# 3. En 'settings.py'

DEBUG = False
ALLOWED_HOSTS = ['*']

import dj_database_url
from decouple import config
DATABASES = {
  'default': dj_database_url.config(
      default=config('DATABASE_URL')
  )
}

# En middleware agregar

'whitenoise.middleware.WhiteNoiseMiddleware',

# Al final

STATICFILES_STORAGE = 'whitenoise.storage.CompressedStaticFilesStorage'

import os

STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
STATIC_URL = '/static/'

# os.makedirs(STATIC_TMP, exist_ok=True)

# os.makedirs(STATIC_ROOT, exist_ok=True)

STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static'),
]

# 6. En 'urls.py'

from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
  path('admin/', admin.site.urls),
  path('', include('website.urls')),
] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

# 7. En Raíz

# crear carpeta

'static'

# crear dentro el archivo

'.keep'

# crear archivo 'Procfile' con

web: gunicorn aqui_nombre_del_proyecto.wsgi --log-file -

# 8. consola

python manage.py collectstatic --noinput

# 10. En consola

# eliminar Pipfile si se usa Pipenv

heroku login
heroku create nombre_de_proyecto
heroku git:remote -a nombre_de_proyecto
git remote -v

heroku addons:create heroku-postgresql:hobby-dev

  git init
  git add .
  git commit -m "estamos deployando, wey"

git push heroku master

heroku run python manage.py migrate

heroku local
<http://0.0.0.0:5000/>

# 11. En cosola (actualizar)

git checkout -b main
git branch -D master
git push heroku main
