---
title: 'Build a CRUD App in Django with Class based Views'
excerpt: "A Tutorial on how to build a simple CRUD app in Django, using function based views and how to transition from function based views to class based views."
comments: true
categories:
  - Software-Engineering
tags:
  - Python
  - Django
---

Function based views used to be the usual way of building applications in Django. This 
was ok but sometimes it invovles repetition of common operations that most web applications use. Recent versions of Django introduced class based views which makes it easy to perform common web application tasks like create, read, update and delete etc.  
Class based views promotes rapid application development by reducing the amount of code needed to perform common tasks.
In this tutorial we will build a simple CRUD app called Daily Journal. We will first build using the usual function based views and then transition step by step to class based views. Using this approach will help us understand and also realise the advantages of class based views. Lets begin!

# Setup

First install django by runing the command below in the terminal.
> It is considered a good practice to create a [virtual environment](https://virtualenvwrapper.readthedocs.io/en/latest/) for your python projects. 

```console
$ pip install django
```
This will install django admin which is a CLI tool for Django apps.

##  Start project
After installing django successfully we will create a django project.
```console
$ django-admin startproject daily_journal
```
This will create a daily_journal directory containing this.
```console 
daily_journal  manage.py
```

```daily_journal``` contains files of our project like settings, urls etc.
```manage.py``` is a python script for managing our app development.  

We then create a ```journal``` app

```console
$ python manage.py startapp journal
```
Our project should have a new ```journal``` directory.
```console 
daily_journal journal manage.py
```

we need to add ```journal``` to our installed apps in settings so that django will be
aware of it.  
In our daily_journal subdirectory lets open the settings file and add ```journal``` to the ```INSTALLED_APPS``` list.

```python
# file: daily_journal/daily_journal/settings.py
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'journal' # add journal app 
]
```
<br>
# Add Models

Our app is a simple daily journal app which will require  title of the journal, content of the journal and a published date.  
Open ```models.py``` in the journal directory and add.

```python
# file: daily_journal/journal/models.py
from django.db import models
from django.urls import reverse
from datetime import date

# Create your models here.
class Journal(models.Model):
    title = models.CharField("Journal Title", max_length=200)
    content = models.TextField()
    published_on = models.DateField(default=date.today) #using current day as default

    def __str__(self):
        return self.title

    def get_absolute_url(self):
        return reverse('show-journal', kwargs={'pk': self.pk})

```
Our model has three fileds and two methods.

```title``` is  a character field with maximum length of 200.  
```content``` is a text field for the content of our journal.  
```published_on``` is a date field with the current day as default.  

```__str__``` helps return a title for our journal when accessed with the console or through django admin app.  

```get_absolute_url(self)``` will be used to access a specific journal with the url.

> Django will create an auto increament primary key ```pk``` for the model.

## Add journals with django admin
Lets interact with the ```Journal``` model by inserting data using django admin.

### Register Models to django admin
Lets register the ```Journal``` model to django admin.

add this code to ```daily_journal/journal/admin.py``` .

```python
# file: daily_journal/journal/admin.py
from django.contrib import admin
from .models import Journal

# Register your models here.
admin.site.register(Journal)
```

### Make migration

Our model repesents the data layer of our application which corresponds to a database
table. We will need to make migration so that django will create the required tables
from our model. Django also comes with other models for user management which also require
migration.

Make migration. 
```console
$ python manage.py makemigrations 
```
The command above will create the needed sql statements for creating the tables

Migrate.
```console
$ python manage.py migrate
```
This will create the actual tables for the models.  
Django comes with sqlite database already setup. We will use this database for our app.  
After migrating you should see a ```db.sqlite3``` file in the root directory.

### Create Super User
We will need to create a super user before using django admin.

create a super user.
```console
$ python manage.py createsuperuser
```
We will be prompted to enter a username, email and a password.

>your password should be minimum of 8 characters containing a capital and a number.

run server

```console
$ python manage.py runserver
```
goto ```http://127.0.0.1:8000/admin``` (displayed in you terminal).  

We should see this page in the browser. Login in with the super user credentials.

{% raw %}<img src="/assets/images/django-admin.png" alt="">{% endraw %}


After loging in. We can add journals from this.

{% raw %}<img src="/assets/images/django-admin-page.png" alt="">{% endraw %}

go ahead and add a couple of journals.

# Read Journals
At this section we will read and display the journals we entered.

lets add a these functions  in ```views.py``` 

>The other functions without the body will be completed as we go on

```python
from django.shortcuts import render, redirect, get_object_or_404
# redirect and get_object_or_404 modules will be used later.
from .models import Journal


def list_journals(request):
    context = {'journals': Journal.objects.all}
    return render(request, 'journal/list_journals.html', context)

def add_journal(request):
    pass

def show_journal(request, pk):
    pass
    

def edit_journal(request, pk):
    pass

def delete_journal(request, pk):
    pass

```

In the ```list_journals``` function we create a dictionary and asign all the journals we query to the journals key. This will make the objects available in the context of the template used to display the list of journals.

lets add the ```list_journals``` template.  
Create a ```templates``` directory in journal and then create another directory called ```journal``` .

In the journal directory, create the html file ```list_journals.html``` with this content.
#### ```file: daily_journal/journal/templates/journal/list_journals.html ```
```html
{% raw %}

<!DOCTYPE html>
<html lang="en">
    <head>
        <title></title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
        <h4>List Journals</h4> 
        <a href="{% url 'add-journal' %}">Add Journal</a>
        <div>
            <ul>
            {% for journal in journals %}
                <li> 
                    <a href="{% url 'show-journal' journal.pk %}">
                         {{ journal.title }} | {{ journal.published_on }}
                    </a>
                    [ <a href="{% url 'edit-journal' journal.pk %}">Edit</a>
                    |
                    <a href="{% url 'delete-journal' journal.pk %}">Delete</a>]
                     
                </li>              
            {% endfor %}
                
            </ul>
        </div>
    </body>
</html>
{% endraw %}
```

## Add url for diplaying list of journals

Create a ```urls.py``` file in the journal directory and add.

```python
# file: daily_journal/journal/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.list_journals, name='list-journals'),
    path('add-journal', views.add_journal, name='add-journal'),
    path('<int:pk>/show-journal', views.show_journal, name='show-journal'),
    path('<int:pk>/edit-journal', views.edit_journal, name='edit-journal'),
    path('<int:pk>/delete-journal', views.delete_journal, name='delete-journal')
]
```
Naming the urlpattern will make it possible to generate the right urls using redirect, reverse or url functions.  
We will use {% raw %} ```{% url 'path-name' %}``` 
{% endraw %} in the template.


Lets include this urls in the daily_journal's urls list.

Locate ```urls.py``` in the ```daily_journal``` folder and change it to this.

```python
# file: daily_journal/daily_journal/urls.py
from django.contrib import admin
from django.urls import path,include # import include module

urlpatterns = [
    path('', include('journal.urls')), # add journal urls
    path('admin/', admin.site.urls),
]

```
goto ```http://127.0.0.1:8000/``` you should see your list of jounals.

{% raw %}<img src="/assets/images/list-journals.png" alt="">{% endraw %}

# Add Journal
Next, Lets add a function for creating journals.  
We would like to show a form to be filled with the title and content of the journal so
lets create that first.
Create a ```forms.py``` in the journal directory and add this.
```python
# file: daily_journal/journal/forms.py
from django import forms

class JournalFoam(forms.Form):
    title = forms.CharField(label="journal form", max_length=200)
    content = forms.CharField(widget= forms.Textarea)
```
This will help create and validate a text input and textarea field in the template.

In the ```views.py``` add this to ```add_journal``` function.

```python
# file: daily_journal/journal/views.py
from .forms import JournalFoam # import the form


def add_journal(request):

    if request.method == 'POST':
        form = JournalFoam(request.POST)
        if form.is_valid():
            title = form.cleaned_data['title']
            content = form.cleaned_data['content']

            Journal.objects.create(title=title, content=content)

            return redirect('list-journals')

    context = {'form': JournalFoam()}
    return render(request, 'journal/add_journal.html', context)
```

In the ```add_journal``` function we first check the request method if it's
a ```POST``` request and then create a form instance from the submitted inputs.
After we check if the form is valid and then process it if it's valid, if not we 
show the form page with the errors.
When request method is not a ```POST``` request we just create a form instance and display the form by passing it to the template.

Create the add_journal template ```templates/journal/add_journal.html``` with this content.

```html
{% raw %}
<!DOCTYPE html>
<html lang="en">
    <head>
        <title></title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
    <h4>Add Journal</h4>
        <form action="{% url 'add-journal' %}" method="POST">
            {% csrf_token %}
            {{ form.as_p }}
            <input type="submit" value="save journal">
        </form>
    </body>
</html>
{% endraw %}

```

goto ```http://127.0.0.1:8000/add-journal``` you should see the form.

{% raw %}<img src="/assets/images/add-journal-form.png" alt="">{% endraw %}
Try it out.

# Display a Journal

Now we would like to display the details of a specific journal.

update  ```show_journal``` function in ```views.py```.

```python
def show_journal(request, pk):
    journal = get_object_or_404(Journal, pk=pk)
    return render(request, 'journal/show_journal.html', {'journal': journal} )
```
In this function we get the primary key ```pk``` as a parameter from the url.
The journal with such primary key is queried. 
If the journal is found we display it, else we show a 'Not Found  page'.

Create the show_journal template ```templates/journal/show_journal.html``` with this content.

```html
{% raw %}
<!DOCTYPE html>
<html lang="en">
    <head>
        <title></title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
        <a href="{% url 'list-journals'  %}"> &lt; &lt; back</a>
        <h4>{{ journal.title }}</h4>
        <p>{{ journal.content }}</p>
    </body>
</html>
{% endraw %}
```
 
Remember the ``` get_absolute_url``` method on our model ?  
Lets update the ```add_journal``` function in ```views.py``` so that it 
shows the details of a journal after adding it.

```python
from .forms import JournalFoam # import the form


def add_journal(request):

    if request.method == 'POST':
        form = JournalFoam(request.POST)
        if form.is_valid():
            title = form.cleaned_data['title']
            content = form.cleaned_data['content']

            journal = Journal.objects.create(title=title, content=content) # assign to a journal variable

            return redirect(journal.get_absolute_url()) # call method to return url

    context = {'form': JournalFoam()}
    return render(request, 'journal/add_journal.html', context)
```
Calling the ```get_absolute_url``` method returns a url string to redirect to after adding a journal.
Try it out. Add a new journal

# Edit Journal
Next,  we add a function for editing a journal.  

Add this to ```edit_journal``` function in ```views.py``` .

```python
def edit_journal(request, pk):
    journal = get_object_or_404(Journal, pk=pk)
    
    if request.method == 'POST':
        form = JournalFoam(request.POST)
        if form.is_valid():
            journal.title= form.cleaned_data['title']
            journal.content = form.cleaned_data['content']

            journal.save()
            return redirect('list-journals')
    
    form = JournalFoam(initial={'title': journal.title,'content': journal.content})

    return render(request, 'journal/edit_journal.html', 
                    {'form': form, 'pk': pk})

```
In ```edit_journal``` function we first get the journal object using the primary key ```pk```
as parameter from the url. If the request method is ```POST``` , the form is proccessed and the changes are saved.  
If the request method is not a ```POST``` request, a new form instance is created with the title and 
content set to that of the journal object queried from the database. The edit journal template is displayed with the form pre-filled.  

Create the show_journal template ```templates/journal/show_journal.html``` with this content.
```html
{% raw %}
<!DOCTYPE html>
<html lang="en">

<head>
    <title></title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>

<body>
    <h4>Edit Journal</h4>
    <form action="{% url 'edit-journal' pk %}" method="POST">
        {% csrf_token %} 
        {{ form.as_p }}
        <input type="submit" value="update journal">
    </form>
</body>

</html>
{% endraw%}
```
Go to the list of journals and click edit to try it out.


# Delete Journal

At this point we can:  
* List Journals
* Add a Journal
* Show a Journal's details
* Edit a Journal

Lets add a function to delete a Journal.

Update the ```delete_journal``` function in ```views.py``` by adding this.

```python
def delete_journal(request, pk):
    journal = get_object_or_404(Journal, pk=pk)

    if request.method == 'POST':
        journal.delete()
        return redirect('list-journals')

    return render(request, 'journal/journal_confirm_delete.html',
                  {'journal': journal})
```
In the  ```delete_journal``` function we first get the journal object with primary key ```pk``` or display a Not found page if it's not available.  
If the request method is a ```POST``` request the journal is deleted else a confirmation page is displayed. 


Create the  template ```templates/journal/journal_confirm_delete.html``` with this content.

```html
{% raw %}
<!DOCTYPE html>
<html lang="en">

<head>
    <title></title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>

<body>
    <form action="{% url 'delete-journal' journal.pk %}" method="POST">
        {% csrf_token %}
        <p>Are you sure you want to delete "{{ journal.title }}"?</p>
        <a href="{% url 'list-journals'  %}"> &lt; &lt; back</a>
        <input type="submit" value="Confirm" />
    </form>
</body>

</html>
{% endraw %}
```
We should be able to delete journals now. Go to the list of journals and click delete.

# Transition to class based views.
Now that we have all the CRUD operations in place, we will convert each function
based view to class based view one after the other.

## Convert  ```list_journals``` view function

Replace the ```list_journals``` function in ```views.py``` with this.

```python
from django.views.generic import ListView # dont forget to import ListView

class JournalListView(ListView):
    model = Journal
    template_name = 'journal/list_journals.html'
    context_object_name = 'journals'
```
As you can see we just specified a few variables.
* ```model``` is asigned the model from which this list will be queried.  
* ```template_name``` is assigned the template name to display the list.  
* ```context_object_name``` is the name of the list object passed to the template.

Now lets link this view to the url pattern. 

Update the ```list-journal``` url in journal's ```urls.py```.

```python

urlpatterns = [
    path('', views.JournalListView.as_view(), name='list-journals'),# update this
    path('add-journal', views.add_journal, name='add-journal'),
    path('<int:pk>/show-journal', views.show_journal, name='show-journal'),
    path('<int:pk>/edit-journal', views.edit_journal, name='edit-journal'),
    path('<int:pk>/delete-journal', views.delete_journal, name='delete-journal')
]

```
Try it out. All should work as usual.  

## Convert ```add_journal``` view function

Replace the ```add_journal``` function in ```views.py``` with this.

```python
from django.views.generic import ListView,CreateView # dont forget to import CreateView

class JournalCreateView(CreateView):
    model = Journal
    template_name = "journal/add_journal.html"
    fields = ['title', 'content']

```
In ```JournalCreateView``` we specified the ```model``` and ```template_name``` to use. We also specified ```fields```, this indicates the fields that needs to be provided to create a journal. We don't need to create or validate a form or check the request method like we did before. All these features are provided automatically. Sweet right?

Lets link it in the urls and see how it works.

update ```list-journal``` path in journal's ```urls.py``` to this.

```python

urlpatterns = [
    path('', views.JournalListView.as_view(), name='list-journals'),
    path('add-journal', views.JournalCreateView.as_view(), name='add-journal'), # update this
    path('<int:pk>/show-journal', views.show_journal, name='show-journal'),
    path('<int:pk>/edit-journal', views.edit_journal, name='edit-journal'),
    path('<int:pk>/delete-journal', views.delete_journal, name='delete-journal')
]

```
Try it. We should be able to add a journal as usual.

## Convert ```show_journal``` view function

Replace the ```show_journal``` function in ```views.py``` with this.

```python
from django.views.generic import (ListView,CreateView,
                              DetailView )# dont forget to import CreateView

class JournalDetailView(DetailView):
    model = Journal
    template_name = 'journal/show_journal.html'
    context_object_name = 'journal'
```
As we've already done, we specify ```model```, ```template_name``` and ```context_object_name``` .
The primary key ```pk``` will be used to fetch and return the journal object for us automatically.
We don't need to do any more work.

Update the urls and try it out.

```python

urlpatterns = [
    path('', views.JournalListView.as_view(), name='list-journals'),
    path('add-journal', view.JournalCreateView.as_view(), name='add-journal'), 
    path('<int:pk>/show-journal', views.JournalDetailView.as_view(), name='show-journal'),# update this  
    path('<int:pk>/edit-journal', views.edit_journal, name='edit-journal'),
    path('<int:pk>/delete-journal', views.delete_journal, name='delete-journal')
]

```

## Convert ```edit_journal``` view function

Replace the ```show_journal``` function in ```views.py``` with this.

```python
from django.views.generic import (ListView,CreateView,
                              DetailView,UpdateView)# dont forget to import CreateView

class JournalUpdateView(UpdateView):
    model = Journal
    context_object_name = 'journal'
    template_name = 'journal/edit_journal.html'
    fields = ['title', 'content']
```

In ```JournalUpdateView``` we specified the required variables as before. 
```fields``` will be used for form processing like in ```JournalCreateView```.
The ```pk``` parameter in url is used to get the journal object to be updated.  

Update the urls to use the class based view and try it out.

```python

  path('', views.JournalListView.as_view(), name='list-journals'),
  path('add-journal', view.JournalCreateView.as_view(), name='add-journal'), 
  path('<int:pk>/show-journal', views.JournalDetailView.as_view(), name='show-journal'),
  path('<int:pk>/edit-journal', views.JournalUpdateView.as_view(), name='edit-journal'),# update this  
  path('<int:pk>/delete-journal', views.delete_journal, name='delete-journal')

```

## Convert ```delete_journal``` view function


Replace the ```delete_journal``` function in ```views.py``` with this.

```python
from django.urls import reverse_lazy # import reverse_lazy
from django.views.generic import (ListView,CreateView,DetailView,
                                 UpdateView, DeleteView)# dont forget to import DeleteView

class JournalDeleteView(DeleteView):
    model = Journal
    context_object_name = 'journal'
    success_url = reverse_lazy('list-journals')
```
Again we specify the ```model``` and ```context_object_name```. The ```success_url``` will be the url to 
redirect to after successfully deleting the journal object. We used ```reverse_lazy``` to make sure
the <b>URLConf</b> is loaded before reversing the url. [docs](https://docs.djangoproject.com/en/2.0/ref/urlresolvers/#django.urls.reverse_lazy)  
The confirmation template follows a conversion,  
ie. ```model-name_confirm_delete.html```.  

In this case the confirmation template name will be ```journal_confirm_delete.html```.  
Update the urls and try it out.


```python

  path('', views.JournalListView.as_view(), name='list-journals'),
  path('add-journal', view.JournalCreateView.as_view(), name='add-journal'), 
  path('<int:pk>/show-journal', views.JournalDetailView.as_view(), name='show-journal'),
  path('<int:pk>/edit-journal', views.JournalUpdateView.as_view(), name='edit-journal'),  
  path('<int:pk>/delete-journal', views.JournalDeleteView.as_view(), name='delete-journal')# update this

```

# Function based views Vs. Class based views.

Now lets compare the two view versions.

Complete function based views.

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Journal
from .forms import JournalFoam

# Create your views here.

def list_journals(request):
    context = {'journals': Journal.objects.all}
    return render(request, 'journal/list_journals.html', context)

def add_journal(request):

    if request.method == 'POST':
        form = JournalFoam(request.POST)
        if form.is_valid():
            title = form.cleaned_data['title']
            content = form.cleaned_data['content']

            Journal.objects.create(title=title, content=content)

            return redirect('list-journals')

    context = {'form': JournalFoam()}
    return render(request, 'journal/add_journal.html', context)

def show_journal(request, pk):
    journal = get_object_or_404(Journal, pk=pk)
    return render(request, 'journal/show_journal.html', {'journal': journal} )

def edit_journal(request, pk):
    journal = get_object_or_404(Journal, pk=pk)
    
    if request.method == 'POST':
        form = JournalFoam(request.POST)
        if form.is_valid():
            journal.title= form.cleaned_data['title']
            journal.content = form.cleaned_data['content']

            journal.save()
            return redirect('list-journals')
    
    form = JournalFoam(initial={'title': journal.title,'content': journal.content})

    return render(request, 'journal/edit_journal.html', 
                    {'form': form, 'pk': pk})

def delete_journal(request, pk):
    journal = get_object_or_404(Journal, pk=pk)

    if request.method == 'POST':
        journal.delete()
        return redirect('list-journals')

    return render(request, 'journal/journal_confirm_delete.html', {'journal': journal})

```

Complete Class based views.

```python
from django.urls import reverse_lazy
from django.views.generic import (ListView, CreateView, DetailView, UpdateView,                                             DeleteView)
from .models import Journal

# Create your views here.

class JournalListView(ListView):
    model = Journal
    template_name = 'journal/list_journals.html'
    context_object_name = 'journals'


class JournalCreateView(CreateView):
    model = Journal
    template_name = "journal/add_journal.html"
    fields = ['title', 'content']


class JournalDetailView(DetailView):
    model = Journal
    template_name = 'journal/show_journal.html'
    context_object_name = 'journal'


class JournalUpdateView(UpdateView):
    model = Journal
    context_object_name = 'journal'
    template_name = 'journal/edit_journal.html'
    fields = ['title', 'content']


class JournalDeleteView(DeleteView):
    model = Journal
    context_object_name = 'journal'
    success_url = reverse_lazy('list-journals')

```
# Conclusion
We can obviously spot the difference. The class based views handled most of the work for us.
That's what we want, to do more with less code.
There are other built-in class based views. eg
* RedirectView
* FormView
* TemplateView etc
check the [docs](https://docs.djangoproject.com/en/2.0/ref/class-based-views/) for a complete of list
available views and use them to your advantage.