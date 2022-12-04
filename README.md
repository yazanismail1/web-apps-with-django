# Creating Web-Apps with Django

1. **Create a project**
    ```
    $ django-admin startproject <project_name> .
    ```
2. **Create a super user aka admin**
    ```
    python manage.py createsuperuser
    ```
3. **Create a templates folder in the root directory**

4. **Register the templates in the project (<projeact_name>/settings.py)**

    in the DIRS:
    ```
    BASE_DIR/"templates"
    ```
5. **Define the static files in the project (<projeact_name>/settings.py)**

    under STATIC_URL constant variable:
    ```
    STATICFILES_DIRS = [BASE_DIR/'static']
    ```
6. **Create a media directory in the root directory**

7. **Create an upload directory inside the media directory**

8. **Define the media in the project (<projeact_name>/settings.py)**

    under STATICFILES_DIRS constant variable:
    ```
    import os
    MEDIA_URL = 'media/'
    MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
    ```
9. **Change the time zone for in the project (<projeact_name>/settings.py)**

    Change the TIME_ZONE constant variable to:
    ```
    'Asia/Qatar' # for UTC+3
    ```
    checkout the following link for all time zones :
    <https://en.wikipedia.org/wiki/List_of_tz_database_time_zones>

10. **Create Apps**
    ```
    $ django-admin startapp <app_name>
    ```
11. **Register the Apps in the project (<project_name>/urls.py)**
    ```
    from django.urls import include
    path('<path>', include('<app_name>.urls'))
    ```
12. **Add the Apps in the project (<projeact_name>/settings.py)**

    in the INSTALLED_APPS:
    ```
    '<app_name>'
    ```
13. **Create the Apps views (<app_name>/views.py)**
    ```
    from django.views.generic import TemplateView

    class <views_class_name>(TemplateView):
        template_name = '<HTML_name>.html'
    ```
    OR
    ```
    from django.views.generic import ListView
    from .models import <model_class_name>

    class <views_class_name>(ListView):
        template_name = '<HTML_file>.html'
        model = <model_class_name>
        context_object_name = '<name>' # to change from object_list to <name> in the templates.

    class Meta:
        verbose_name_plural = "<table_name>"
        ordering = ['-pk'] # to change the ordering displayed
    ```
    OR
    ```
    from django.views.generic import DetailView
    from .views import <model_class_name>

    class <views_class_name>(DetailView):
        template_name = '<HTML_file>.html'
        model = <model_class_name>
        context_object_name = '<name>' # to change from object_list to <name> in the templates.

    class Meta:
        verbose_name_plural = "<table_name>"
        ordering = ['-pk'] # to change the ordering displayed
    ```
14. **Create the urls for the Apps**
    
    Create new directory inside the app called (urls.py):
    ```  
    from django.urls import path
    from django.conf import settings
    from django.conf.urls.static import static
    from .views import <views_class_name>
    
    urlpatterns = [
        path('<path>', <views_class_name>.as_view(), name='<name>')
        ]
    if settings.DEBUG:
        urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    ```
15. **Create the models aka the DB tables**
    ```
    from django.contrib.auth import get_user_model

    class <model_class_name>(models.Model):
        name = models.CharField(max_length=int, help_text=str, default=str)
        rank = models.IntegerField()
        author = models.ForeignKey(get_user_model(), on_delete=models.CASCADE)
        time = models.DateTimeField()
        description = models.TextField()
        img = models.ImageField(upload_to='upload/')
    
        def __str__(self):
            return self.name
    ```
16. **Register the models in the admin (<app_name>/admin.py)**
    ```
    from .models import <model_class_name>
    
    admin.site.register(<model_class_name>)
    ```
17. **Update the DB**
    ```
    python manage.py makemigrations
    python manage.py migrate
    ```
18. **Create the tests for the apps (<app_name>/tests.py)**
    ```
    from django.test import SimpleTestCase or TestCase
    from django.urls import reverse
    
    class <test_class_name>(SimpleTestCase or TestCase):
        def <test_method_name>_status(self):
            url = reverse('<name> from step 14')
            response = self.client.get(url)
            self.assertEqual(response.status_code, 200)
    
        def <test_method_name>_template(self):
            url = reverse('<name> from step 14')
            response = self.client.get(url)
            self.assertTemplateUsed(response, '<HTML_name>.html')
    ```
19. **Create the HTML | Example**
    ```
    {% load static %}
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link rel="stylesheet" href="{% static 'css/reset.css' %}" />
        <link rel="stylesheet" href="{% static 'css/home.css' %}" />
        <title>Django Snacks</title>
    
    </head>
    <body class="home-body">
        {% include 'header.html' %}
        <div class="home-header">
            <div class="background"></div>
            <h1>TRACK YOUR SNACKS!</h1>
        </div>
        <div class="body-cards">
            {% for snack in object_list %}
                <div class="snack-card">
                    <img src='{{snack.img.url}}' width="250" height="250"/>
                    <div class="card-details">
                        <h3>{{ snack.name }}</h3>
                        <hr>
                        <p>{{snack.summary}}</p>
                        <p class="time">{{snack.time}}</p>
                        <a href="{% url '<name> from step 14' snack.id %}">Read More!</a>
                    </div>
                </div>
            {% endfor %}
        </div>
    </body>
    </html>
    ```

**General Commands**

1. **To run the server**
    ```
    python manage.py runserver
    ```
2. **To run the server on a specific port**
    ```
    python manage.py runserver <int: port_number>
    ```
3. **To kill the port**
    ```
    kill -9 $(lsoft -t -l:"8000"
    ```
4. **To run the tests**
    ```
    python manage.py test
    ``` 
