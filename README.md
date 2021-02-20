# Django Interview Preparation
## 1. Types of inheritance in Django
There are three types of inheritance that Django supports
- Abstract Base Class
- Multi-table Inheritance
- Proxy Models

<b>Abstract Base Class</b>
1. Parent Class has attributes common for many child classes
2. Parent Class is only used for inheritance, not saved in database
3. In Parent's Meta class, 'abstract' is marked as True
4. Child class can inherit from many parent classes. 
5. Parent class can't be used for creating objects

```python
class Person(models.Model):
    # common fields
    class Meta:
        abstract = True

class Student(Person):
    pass

class Teacher(Person):
    pass

Person() #Not callable|Can't create objects
```
<b>Multi-table inheritance</b>
1. Every model is a model in itself.
2. One-to-One link is created b/w tables automatically.

```python
class Student(models.Model):
    # Student Attributes
    pass

class Teacher(Student):
    # Teacher inherits the properties of Student
    pass
```
<b>Proxy Models</b>
1. Proxy(<i>something authorized to act on behalf of another</i>) models altering some properties of base model like ordering or adding new method
2. Changes the behavior of original model, won't be saved in DB
3. Proxy model will also operate on the base model

```python
class Person(models.Model):
    pass

class MyPerson(Person):
    class Meta:
        proxy = True
        ordering = ["last_name"]

    def do_something(self):
        pass
```

## 2. What is a manage.py file in Django? List some commands
Manage.py file is automatically created when a new project is created & is used for administrative tasks. <br>
It is more like django-admin but also sets DJANGO_SETTINGS_MODULE for project settings(which db, middleware)
| Command | Function | Example |
| :-------------:|:-------------:| :-------------:| 
|  runserver   | Starts a lightweight dev server on local machine on port 8000 and ip 127.0.0.1. Automatically reloads code on each HTTP request. Can provide local machine ip to make project accessible to other users. | ``` $ python manage.py runserver 192.168.1.110:8000 ``` |
|  check   | Check apps for common problems | ``` $ python manage.py check appname ``` |
|  dumpdata   | Returns all data within all apps on shell. Can be exported to a json, also can be exported for an app, all tables or a single table.  | ``` $ python manage.py dumpdata appname && python manage.py dumpdata appname > appname.json && python manage.py dumpdata appname.model_class_name ``` |
| loaddata | Loads named fixtures into DB | ```$ python manage.py loaddata fixture_name.json``` |
| flush | Irreversibly destroy data currently in DB & return each table to an empty state | ``` $ python manage.py flush ``` |
| makemigrations | Migrations are kind of a version control system for DB schema. makemigrations pack up the changes detected in the models into migration files(similar to a commit). </br>New migrations are compared with the old migrations before creation. | ```python $ python manage.py makemigrations --name changed_my_model appname1, appname2 ``` |
| migrate | Synchronizes the database state with the current set of models and migrations | ``` $ python manage.py migrate changed_my_model --fake ```|

## 3. What is a Fixture?
Fixtures are initial data for your database which can be used with <i>manage.py loaddata</i> command. Django will look for a fixtures folder in your app or the list of directories provided in the FIXTURE_DIRS in settings, and use its content instead.

## 4. Explain Migration in Python Django.

## 5. Difference Between a Project & App?

## 6. How do we initialize a project & an app?

## 7. What does the settings.py file do?

## 8. How do you set up a database connection?

## 9. Django Templating Language

## 10. What are CSRF Tokens?

## 11. What are Django Signals?

## 12. How can we set restrictions on views?

## 13. Explain transaction in Django

## 14. What is the use case of REST API? Why should we use it?

## 15. Explain Serialization. 

## 16. Explain all the files that are created within a new project.

## 17. Explain Django Architecture. 

## 18. Explain Sessions in Django
