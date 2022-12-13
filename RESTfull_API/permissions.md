1. **Install restframework**

    ```
    pip install djangorestframework
    ```

2. **Inside <project_name>/settings.py add rest framework to the installed apps**

    ```
    INSTALLED_APPS = [
        'rest_framework',
    ]
    ```

3. **Inside <project_name>/settings.py add rest framework permissions**

    > See [Django REST framework](https://www.django-rest-framework.org/api-guide/permissions/#api-reference) for more details on types of permissions. The method that is applied here, is called a project authentication.

    ```
    REST_FRAMEWORK = {
        'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.<any_method>',
        ]
    }
    ```

4. **Inside <project_name>/urls.py add the urls**

    ```
    from django.contrib import admin
    from django.urls import path,include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('api/v1/things/',include('<app_name>.urls')),
        path('api-auth/', include('rest_framework.urls'))
    ]
    ``` 
    > **path('api-auth/', include('rest_framework.urls'))**, is a built-in app in django rest framework and is reponsible for the sign in functionality.

5. **Inside <app_name>/model.py create your model**

    ```
    from django.db import models
    from django.contrib.auth import get_user_model

    class <model_class>(models.Model):
        owner=models.ForeignKey(get_user_model(),on_delete=models.CASCADE)
        name=models.CharField(max_length=255)
        description=models.TextField(blank=True)
        created_at=models.DateTimeField(auto_now_add=True)
        updated_at=models.DateTimeField(auto_now=True)

        def __str__(self):
            return self.name
    ``

6. **Inside the <app_name>/admin.py register the model**

    ```
    from django.contrib import admin
    from .models import <model_class>
    
    admin.site.register(<model_class>)
    ```

7. **Create a new directory <app_name>/serializers.py**

    This file will contain a class that requires a model, its main use and benefit is to convert the data from the model in the DB to a JSON form so that a user can apply API logic to it.

    ```
    from rest_framework import serializers
    from .models import <model_class>

    class <name>Serializer(serializers.ModelSerializer):
        class Meta:
            model= <model_class>
            # fields=['id','name', 'owner', 'description']
            fields='__all__'
    ```

    - **fields = '_ _ _all_ _ _'**

        Will return all the model's fields, we can specify which fields we want by putting them in an array in a string format.

8. **Create a new directory <app_name>/permissions.py**

    > The following class is to create a custom permission, it can be skipped and you can use a built in permission class.

    ```
    from rest_framework import permissions

    class IsOwnerOrReadOnly(permissions.BasePermission):
        def has_object_permission(self, request, view, obj):
            if request.method == 'GET' :
                return True
            if  request.user==obj.owner:
                return True
            else:
                return False  
    ```

9. **Inside <app_name>/views.py add the views**

    ```
    from django.shortcuts import render

    from rest_framework.generics import ListAPIView, ListCreateAPIView, RetrieveAPIView, RetrieveUpdateAPIView, RetrieveUpdateDestroyAPIView

    from .serializers  import <Serializer_class>

    from .models import <model_class>

    from .permissions import IsOwnerOrReadOnly

    from rest_framework.permissions import AllowAny,IsAuthenticatedOrReadOnly


    class ThingListView(ListCreateAPIView):
        queryset=Thing.objects.all()
        serializer_class= <Serializer_class>
        permission_classes=[IsAuthenticatedOrReadOnly]


    class ThingDetailView(RetrieveUpdateDestroyAPIView):
        queryset=Thing.objects.all()
        serializer_class= <Serializer_class>
        permission_classes=[IsOwnerOrReadOnly]

    ```
    - **ListAPIView**

        This class lists all the objects in the DB (`GET` request).

    - **ListCreateAPIView**

        This class lists all the objects in the DB and allow creating new objects (`GET` and `POST` requests).

    - **RetrieveAPIView**

        This class return a specific object from the DB (`GET` request).

    - **RetrieveUpdateAPIView**

        This class return a specific object from the DB and allow updating the object (`GET`, `PUT` and `PATCH` requests).

    - **RetrieveUpdateDestroyAPIView**

        This class return a specific object from the DB and allow updating and deleting the object (`GET`, `PUT`, `PATCH` and `DELETE` requests).

    - **IsOwnerOrReadOnly**

        This class allows any one to send a `GET` request, if the request is anything other than `GET` such as `POST`, `PUT`, `PATCH` or `DELETE`, the class will check if the send is the owner of object if so the class will allow the request, otherwise it will decline it.

    - **IsAuthenticatedOrReadOnly**

        This class allows any one to send a `GET` request, if the request is anything other than `GET` such as `POST`, `PUT`, `PATCH` or `DELETE`, the class will check if the send is authenticated if so the class will allow the request, otherwise it will decline it and will treat the request as a `GET` request.

     - **AllowAny**

        This class allows any one to send any kind of requests such as `GET`, `POST`, `PUT`, `PATCH` or `DELETE`, regardless if the send is authenticated or not.

10. **Create a new directory <app_name>/urls.py and add the urls for the views**

    ```
    from django.urls import path
    from .views import <view_name>, <another_view_name>
    urlpatterns = [
        path('', <name>.as_view(), name='<name>'),
        path('<int:pk>', <another_view_name>.as_view(),name='<name>')
    ]
    ```

11. **Inside the <app_name>/tests.py add the tests**

    > See [REST APIs Permissions Testing](RESTfull_API/testing.md) for the details of testing.














