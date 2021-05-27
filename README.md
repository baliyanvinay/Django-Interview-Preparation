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
- Model with a dependency is created first, say Teacher has foreign key to Person model, the Person model is created first
- Each app has migrations stored in migration file with class representation. Django.db.migrations.Migration is the migration class
- Each migration class lists dependencies(a list of mig this depends on) and operations(what was changed in models)
- These operations are then used to create the SQL which then makes the changes
- It is possible to write migration files manually without running the 'makemigrations' command
- Initial migrations are marked with an initial = True class attribute on the migration class
- If you have manually changed your db, Django won't be able to migrate changes | errors
- Migrations can be reverted by passing the migration number ``` $ python manage.py migrate app 002 ``` 
- Migrations can also be named ```$ python manage.py makemigrations --name changed_my_model your_app_label ```

## 4a. How does Django knows which migration to apply?
Django creates a 'django_migrations' table in db with fields <ID | App | Name | Applied> when you apply any migration the first time. A new row is inserted for each migration that is applied or faked. 

## 4b. Explain fake migration in Django

## 5. Difference Between a Project & App?
A project is whole idea behind your application and an app is the sub-module of that idea. Example- Users as a micro-service project with profile app for profile related management, authentication app and so on. In a general way, a project could be the whole website and an app could be functionalities it offers, like request app, result app, frontend app and so on. 

## 6. How do we initialize a project & an app?
#### Project
- Create the project ``` $ django-admin.py startproject project_name ```
- Update your project settings in ./settings.py like installed_apps, middlewares, db, project variables & so on.
- Validate installment | use Django dev test server
- Version control project, creates requirements.txt

#### App
- Create the app ``` $ python manage.py startapp app_name ```
- Add app into project settings.py installed_apps
- Make migrations & migrate models

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

## 19. Explain Squashing Migration in Django.
Squashing is the act of reducing an existing set of many migrations down to one (or sometimes a few) migrations which still represent the same changes. Django takes all the operations from the migrations, puts them in sequence and optimizes them. 
- Create model and Delete model --> No operation
- You can name squashed migrations ``` python manage.py squashmigrations myapp --squashed-name squashed_migration_01 ```
- All migration files must be deleted after a squashed migration(leaving the squashed one)
- Update dependencies of deleted migrations to squashed migration. 
- Remove 'replaces' from squashed migration file. 'replaces' is what makes it a squashed migration

## 20. Difference between class based views and function based views.
When to use which one?

## 21. Explain the request flow in Django. 

## 22. Explain middlewares in Django. 
Triggers and all

## 23. Explain mixins. What makes mixins different.

## 24. How to migrate from one database to another?
Use of two setting files.

## 25. Explain bulk_create on post in DRF.
## 26. Explain REST HTTP methods
| Method | CRUD   | Entire Collection       | Specific Item                            |
| :-----:|:------:| :----------------------:| :---------------------------------------:|
| GET    | Read   | 200(OK)                 | 200(OK), 404(Not Found)                  |
| POST   | Create | 201(Created)            | 404(Not found), 409(Conflict)            |
| PUT    | Update | 405(Method Not Allowed) | 200(OK), 204(No Content), 404(Not Found) |
| PATCH  | Modify | 405(Method Not Allowed) | 200(OK), 204(No Content), 404(Not Found) |
| DELETE | Delete | 405(Method Not Allowed) | 200(OK), 404 (Not Found)                 |

### Status Codes
- Informational 1XX
- Successful 2XX
- Redirection 3XX
- Client Error 4XX
- Server Error 5XX

### Difference between a POST and PUT request.
Calling the same PUT request multiple times will always produce the same result. In contrast, calling a POST request repeatedly make have side effects of creating the same resource multiple times.

## 27. How would you create models from legacy DB?
## 28. Difference between Django and Flask
## 29. Explain Middlewares. What methods are required within a middleware?
