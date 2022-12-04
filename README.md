# Creating Django Apps

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
