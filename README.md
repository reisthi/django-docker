# Docker Container + Django app

Simple skeleton for a django project within a docker container


## Getting Started
On an empty project directory, create three files:

Dockerfile 
```FROM python:3
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/
```
docker-compose.yml
```version: '3'

services:
  db:
    image: postgres
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db
```
requirements.txt
```
Django>=2.0,<3.0
psycopg2>=2.7,<3.0
```



## Create Django Project
```bash
sudo docker-compose run web django-admin startproject composeexample .
```

Replace ```DATABASES``` in the ```composeexample/settings.py``` file with the following:

```DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}
```

## Usage

Run docker.

```docker 
docker-compose up
```
Shut down docker.

```docker 
docker-compose down
```
List running docker processes.

```docker 
docker container ls
```
For more info [docs.docker](https://docs.docker.com/compose/django/)