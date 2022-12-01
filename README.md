# Django Quickstart Project

<p align="center">
  <img width="180" height="180" src="https://klauslaube.com.br/static/4ff9b044c4ab9ace735892bea0ab70a1/django-rest-framework-logo.png">
</p>

## What is the pourpose of this repository?

This project is simply the outcome of following the [quickstart section](https://www.django-rest-framework.org/tutorial/quickstart/) of Django REST framework. But to not just be a pointless copy of the quickstart project, I'll do my best to describe what the process in a more friendly approach and also put into words the knowledge I gained from the tutorial.

## Setting up the environment

I'd recommend using some kind of virtual environment like venv or conda env to isolate your dependencies from other projects or global environments and avoid conflicts overall.
In my case, I used a fresh conda environment. To setup a new conda env, you first need to install [Anaconda](https://www.anaconda.com/) and then type the following command in the command line:

`conda create --name <give it a name>`

then

`conda activate <name you gave>`

To make the process easier, I made a setup file, so you just need to copy it and type the following command in the command line (from the root directory):

`pip install .`

This will install Django, django REST framework and pylint into your environment.

## Project setup

Following the django tutorial, we need to create a new project. This is where all the config goes (project settings, database, included middlewares, included apps, available urls and so on). Then we run:

`django-admin startproject tutorial .`

to create a new project named tutorial in the root folder (where we are right now), and inside the newly created folder (a project folder named tutorial will be created, so inside the projects folder) we run:

`django-admin startapp quickstart`

to create a new app named quickstart. This makes a new app folder where we have our models, views, serializers, etc. It's created within the project's directory to avoid name clashes with external modules by using the project's namespace.

Because we haven't touched the `settings.py` file, we'll be using sqlite to store our models and data. Now we need to sync our database for the first time by running:

`python manage.py migrate`

at the root directory.

The tutorial suggests you to create a superuser to ensure security on your database management. Keep in mind that making requests other than GET requests by your browser or whatever you want to use are going to be hard or maybe impossible (I didn't dig too deep to be sure and since it's a starter project with the purpose to present you the framework's core features, I wouldn't think too much about it for now). So we run:

`python manage.py createsuperuser --email admin@example.com --username admin`

and set the password to `password123`.

## Creating serializers

Serializers are classes that allow complex data such as querysets and model instances to be converted to native Python datatypes that can then be easily rendered into JSON, XML or other content types. Serializers also provide deserialization, allowing parsed data to be converted back into complex types, after first validating the incoming data. Django models are too complex for for databases to handle and that's why we need serializers!

Let's start by creating a `serializers.py` inside `tutorial/quickstart/` (our app folder) and writing the following code in it:

```py
"""
We are using example models just for the sake of this tutorial. Normally you would import your own modules from the models.py file
"""
from django.contrib.auth.models import User, Group
from rest_framework import serializers

class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ['url', 'username', 'email', 'groups']


class GroupSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Group
        fields = ['url', 'name']
```

Our serializer classes inherit from `HyperlinkedModelSerializer` because our models use hyperlinks as unique identifiers. In the inner class `Meta` we define the model and the model's fields to be serialized and deserialized. If we wanted all fields, we could pass `'__all__'` to the fields variable.
