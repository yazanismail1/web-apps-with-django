# Custom User Model

1. **Create an app called accounts**

    ```
    python manage.py startapp accounts
    ```

2. **Resigter the app in <project_name>/settings.py**

    ```
    INSTALLED_APPS = [
        ...
        'accounts',
        ...
    ]
    ```

3. **Overwrite the existing user model in the <project_name>/settings.py**

    ```
    AUTH_USER_MODEL='accounts.CustomUser'
    ```

4. **Create the Custom User model in <app_name>/models.py**

    ```
    from django.db import models
    from django.contrib.auth.models import AbstractUser
    
    class CustomUser(AbstractUser):
        pass # or add any extra fields you want.
    ```

5. **Register the model in <app_name>/admin.py**

    ```
    from django.contrib import admin
    from .models import CustomUser

    admin.site.register(CustomUser)
    ```

6. **Customize the admin pannel [Optional] in <app_name>/admin.py**

    ```
    from django.contrib import admin
    from django.contrib.auth.admin import UserAdmin

    class CustomUserAdmin(UserAdmin):
        add_fieldsets=((
            'None',{
                'fields':('username','email','password1','password2'),
            }
        ),(
            'personal information',{
                'fields':('first_name','last_name','phone_number'),
            }
        ),)
    
    admin.site.register(CustomUserAdmin)

    ```
    > Note that the fields must be presented in the custom user model.
    
    > Username, email, first_name, last_name, password1 and password2, are default fields.





















