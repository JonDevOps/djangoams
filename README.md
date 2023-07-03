Virtual Environments FAQ

1. You created a directory in your windows home directory called .virtualenvs to store all your virtual environments

2. Run cd %HOMEPATH% to get to your home direcory in cmd

3.To create a new virtual environment run "python3 -m venv .virtualenvs\nameOfEnv", the path is where the env will be saved

4. Activate the virtualenv by running "%HOMEPATH%\.virtualenvs\nameofEnv\Scripts\activate.bat" (i run this in the home path)
   -activate the virtual env whenever you open a new terminal window!

5. While that virtual env is active, any pip packages you install will be installed there

6. You can install a local version of django you may have downloaded by pointing to your local copy

7. To verify that Django can be seen by Python, type python from your shell. Then at the Python prompt, try
to import Django:
>>> import django
>>> print(django.get_version())


8. Script\deactivate will deactivate the vikrtual env on windows









Django 1st steps part 1

0. After creating the virtual environment and activating it, cd into the directory you would like your projects code to be stored, for example you workspace directory and run:
"django-admin startproject nameOfProj"

Here is what startproject created:
nameOfProj/
 manage.py
 nameOfProj/
  __init__.py
  settings.py
  urls.py
  asgi.py
  wsgi.py
These files are:
• The outer mysite/ root directory is a container for your project. Its name doesn’t matter to Django; you can rename it to anything you like.
• manage.py: A command-line utility that lets you interact with this Django project in various ways.
• The inner mysite/ directory is the actual Python package for your project. Its name is the Python
package name you’ll need to use to import anything inside it (e.g. mysite.urls).
• mysite/__init__.py: An empty file that tells Python that this directory should be considered a Python package.
• mysite/settings.py: Settings/configuration for this Django project. 
• mysite/urls.py: The URL declarations for this Django project; a “table of contents” of your Django powered site. 
• mysite/asgi.py: An entry-point for ASGI-compatible web servers to serve your project. 
• mysite/wsgi.py: An entry-point for WSGI-compatible web servers to serve your project. 

1. Verify if your Django project works. Change into the outer nameOfProj directory and run the following commands: 
"$ python manage.py runserver"

Then, the following will output to the comand line:
"Performing system checks
System check identified no issues (0 silenced)
You have unapplied migrations; your app may not work until they are applied.
Run 'python manage.py to migrate' to apply them.
June 20, 2023 - 15:50:53
Django version 4.2, using settings ams.settings
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C."

This starts the Django development server. 
Do not use this server in production. Its only meant to be used while developing.
Now that the server is running visit http:/127.0.0.1:8000/ with your browser. You will see a congratulations oage, with a rocket taking off. It worked!

To change the servers port run: 
"$ python manage.py runserver 8080"

To change the servers id, pass it along with the port. For example to listen on all available ips use: 
"$ python manage.py runserver 0.0.0.0:8000"

The development server automatically reloads. Only adding new files will require a restart.


2. Now that your environment-a "project"- is set up you're set to start doing work.
Each app you write in Django consists of a Python package that follows a certain convention. Django comes with a file utility that automatically generates the basic directory structure of an app, so you can focus on writing code and not directories.

-KEY TERMS
"App": An app is a web project that does something-e.g., a blog system, a database of public records or a small poll app.
"Project": A project is a collection of configuration and apps for a particular website.

--A project  can contain multiple apps. And an app can be in multiple projects

Your apps can live anywhere on your Python path. In this tutorial, we’ll create our poll app in the same directory as your manage.py file so that it can be imported as its own top-level module, rather than a submodule of nameOfProj.

To create your app, make sure you're in the same directory as manage.py and type the command:
"$ python manage.py startapp polls"

That'll create a directory "polls", which is laid out like this:
polls/
  __init__.py
  admin.py
  apps.py
  migrations/
    __init__.py
  models.py
  tests.py
  views.py

This directory structure will house the polls application.


2. Now to write the simplest example of a view possible in Django first open the file polls/views.py 

Put the following code in it: 
"from django.http import HttpResponse

def index(request):
  return HttpResponse("Hello, world. You're at the polls index.")"

To call the view we need to map it to a URL - and for this we need a URLconf.

To create a URLconf in the polls directory, create a file called urls.py., The app directory should now look like:
polls/
  __init__.py
  admin.py
  apps.py
  migrations/
    __init__.py
  models.py
  tests.py
  urls.py
  views.py

In the polls/urls.py file include the following code
"from django.urls import path

from . import views

urlpatterns = [
    path("", views.index, name="index"),
]"

Next, point the root URLconf at the polls.urls module. In nameOfProj/urls.py add an import for django.urls.include and insert an include() in the urlpatterns list, so you have:
"from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path("polls/", include("polls.urls")),
    path("admin/", admin.site.urls),
]"

The include() function allows referencing other URLconfs. Whenever Django encounters include(), it chops off whatever part of the URL matched up to that point and sends the remaining string to the included URLconf for further processing,

The idea behind include() is to make it easy to plug-and-play URLs. Since polls are in their own URLconf (polls/urls.py), they can be placed under “/polls/”, or under “/fun_polls/”, or under “/content/polls/”, or any other path root, and the app will still work.

You should always use include() when you include other URL patterns. admin.site.urls is the only exception to this.

You have now wired an index view into the URLconf. Verify it’s working with the following command: 
"python manage.py runserver"

Go to http://localhost:8000/polls/ in your browser, and you should see the text “Hello, world. You’re at the polls index.”, which you defined in the index view.