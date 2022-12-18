# Production Server With Docker

1. **Install Relevant Libraries**

    ```
    pip install whitenoise gunicorn
    ```

2. **Update the MIDDLEWARE in <project_name>/settings.py**

    ```
    MIDDLEWARE = [
        'django.middleware.security.SecurityMiddleware',
        "whitenoise.middleware.WhiteNoiseMiddleware",
        ...
    ]
    ```

3. **Enable the read of static files in <project_name>/settings.py**

    ```
    import os

    STATIC_DIR = os.path.join(BASE_DIR, 'static')
    STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
    STATIC_URL = 'static/'
    STATICFILES_DIRS = [
        STATIC_DIR,
    ]
    ```

4. **Create a Dockerfile in root of your directory**

    ```
    FROM python:3
    ENV BUILDKIT_PROGRESS=plain
    ENV PYTHONDONTWRITEBYTECODE 1
    ENV PYTHONUNBUFFERED 1
    RUN mkdir /code
    WORKDIR /code
    COPY requirements.txt /code/
    RUN pip install -r requirements.txt
    COPY . /code/
    ```

5. **Create a docker-compose.yml file in the root of your directory**

    ```
    version: "3.9"

    services:
    web:
        build: .
        command: gunicorn <project_name>.wsgi:application --bind 0.0.0.0:8000 --workers 4
        volumes:
        - .:/code
        ports:
        - "8000:8000"
        depends_on:
        - db

    db:
        image: postgres  
        environment:
        - POSTGRES_DB=postgres
        - POSTGRES_USER=postgres
        - POSTGRES_PASSWORD=postgres
    ```

6. **Update the DB in <project_name>/settings.py to PostgreSQL**

    ```
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql',
            'NAME': 'postgres',
            'USER': 'postgres',
            'PASSWORD': 'postgres',
            'HOST': 'db',
            'PORT':'5432'
        }
    }
    ```

7. **Proceed as normal with Docker Steps**

    > See [Using PostgreSQL with Docker](/PostgreSQL_Docker/PostgreSQL_docker.md) for more details