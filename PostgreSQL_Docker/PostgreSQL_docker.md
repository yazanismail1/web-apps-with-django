1. **Install Psycopg2-binary and all other required libraries/frameworks**

    ```
    pip install psycopg2-binary
    ```

2. **Freeze the requirements**

    ```
    pip freeze > requirements.txt
    ```

3. **On the top level add a new file called Dockerfile**

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

4. **On the top level add a new file called docker-compose.yml**

    ```
    version: "3.9"

    services:
    web:
        build: .
        command: python manage.py runserver 0.0.0.0:8000
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

5. **Inside <project_name>/settings.py replace SQLite with PostgreSQL**

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

6. **Build Docker container**

    ```
    docker-compose up
    ```
    ```
    docker-compose up --build
    ```

7. **While running docker we need to make migrations to get the models and users into our new DB**

    Open new terminal while the other one is running "DON'T STOP IT".
    
    - run `docker-compose run web python manage.py makemigrations`.

    - run `docker-compose run web python manage.py migrate`.

    - run `docker-compose run web python manage.py createsuperuser`.

