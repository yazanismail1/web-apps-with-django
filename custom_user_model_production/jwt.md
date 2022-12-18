# JSON Web Token + DRF

1. **Install Simple JWT, drf and supporting libraries**

    ```
    pip install djangorestframework-simplejwt djangorestframework psycopg2-binary
    ```

2. **Register them in the <project_name>/settings.py**

    ```
    INSTALLED_APPS = [
        ...
        'rest_framework',
        'rest_framework_simplejwt',
        ...
    ]
    ```

3. **Add Default project Permissions and Authentications <project_name>/settings.py**

    ```
    REST_FRAMEWORK = {
        'DEFAULT_PERMISSION_CLASSES': [
            'rest_framework.permissions.IsAuthenticatedOrReadOnly',
        ],
        'DEFAULT_AUTHENTICATION_CLASSES': (
        
            'rest_framework_simplejwt.authentication.JWTAuthentication',
            "rest_framework.authentication.SessionAuthentication",
            "rest_framework.authentication.BasicAuthentication",
        )
   
    }

    ```

4. **Add urls <project_name>/urls.py**

    ```
    from django.contrib import admin
    from django.urls import path,include
    from rest_framework_simplejwt.views import TokenObtainPairView, TokenRefreshView,
    from .jwt_tokens import MyTokenObtainPairView

    urlpatterns = [
        path('admin/', admin.site.urls),
        ...
        path('api-auth/', include('rest_framework.urls')),
        path('api/token/', MyTokenObtainPairView.as_view(), name='token_obtain_pair'),
        path('api/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
        ...
    ]

    ```

5. **Customize JWT Claims | Create a new file <project_name>/jwt_tokens.py**

    ```
    from rest_framework_simplejwt.serializers import TokenObtainPairSerializer
    from rest_framework_simplejwt.views import TokenObtainPairView

    class MyTokenObtainPairSerializer(TokenObtainPairSerializer):
        @classmethod
        def get_token(cls, user):
            token = super().get_token(user)
            # Add custom claims
            token['username'] = user.username
            return token

    class MyTokenObtainPairView(TokenObtainPairView):
        serializer_class = MyTokenObtainPairSerializer
    ```

6. **Create an application and proceed as normal**

    > See [REST APIs Permissions](/RESTfull_API/permissions.md) tutorial for information.






















