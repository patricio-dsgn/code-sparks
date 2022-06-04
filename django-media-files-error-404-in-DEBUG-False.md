# Solucion a archivos MEDIA no encontrados en Debug is False

nota:

Esta es la manera 2 de solucionarlo descrita en [https://bestprogrammingblogs.blogspot.com/2021/03/django-static-files-not-working-when-debug-is-false.html](https://bestprogrammingblogs.blogspot.com/2021/03/django-static-files-not-working-when-debug-is-false.html).

En `djago.urls` existe un problema con `from django.urls import include, url`, esto se soluciona reemplazando con `from django.urls import include, re_path` descrito en [https://stackoverflow.com/questions/70319606/importerror-cannot-import-name-url-from-django-conf-urls-after-upgrading-to](https://stackoverflow.com/questions/70319606/importerror-cannot-import-name-url-from-django-conf-urls-after-upgrading-to).

---

primero en settings.py

``` python
DEBUG = False
ALLOWED_HOSTS = ['*']
```

1. En settings.py

    ```python
    STATIC_URL = '/static/'
    MEDIA_URL = '/media/'


    if DEBUG:
        STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]
    else:
        STATIC_ROOT = os.path.join(BASE_DIR, 'static')
    MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
    ```

2. En urls.py

    arriba de todos los import

    ``` python
    from django.urls import include, re_path
    from django.conf import settings
    from django.views.static import serve
    #from bla bla bla...
    ```

    arriba de las otras url

    ``` python
    urlpatterns = [
        re_path(r'^media/(?P<path>.*)$', serve,{'document_root': settings.MEDIA_ROOT}),
        re_path(r'^static/(?P<path>.*)$', serve,{'document_root': settings.STATIC_ROOT}),
        #url( bla bla bla...
    ]
    ```

3. en consola

    ``` console
    python manage.py runserver
    ```
