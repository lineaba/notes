[Tutorial here](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world)

# Running the application #
- Navigate to the project folder
- Activate virtual environment
```venv\Scripts\activate```
- Set the flask app
```set FLASK_APP = [PROJECT].py```
- Run the app
```flask run```
- Open app in browser
```localhost:5000```

# General Flask project-structure #
```
#Using [PROJECT] as the dummy-name of the project - in implementation you would replace this with an actual project-name  
[PROJECT]/
  venv/             #the virtual environment
  [PROJECT].py      #Main application module - The executed file
  config.py         #Contains the defintion for the config class where secret keys etc. are defined.
  app/              #app-folder
    __init__.py     #imports flask and make a flask instance?
    routes.py       #URL routes and their callback function
    forms.py
    templates/
      base.html     #contains html for elements which should be present in all templates
      login.html    #Html or login webform (extends base.html)
```

# Chapter 1 - Getting started #
### Virtual environment ###
Using a virtual environment means that you can use a specific version of something like flask for one project, while using another version for another project. This is because rather than making an installation for the entire machine, the installation is applied directly to the virtual environment. 
I wonder how this actually works. If I have a bunch of different virtual environment, do I have individual maps saved multiple times, taking up desk space? Or is it more like a git-type system, where the system just keeps track of which changes to apply to which “branch”, assuming that each virtual environment was equivalent to a branch?

```
#Activating a virtual environment    
(assuming it is already installed, and that I have navigated to the correct folder)  

venv\Scripts\activate
```

### Decorators ###
It is kind of a weird name, decorators, becuase it kind of sounds like to me, that they are only there to make it look pretty, whichout actually playing any "real" role.  
But decorators modifies the function that immediately follow them.
Multiple decoraters can modify the same function. Decoraters can be used to create a call-back relationsship between some event and a function that execute on that event.  
The ```@app.route``` creates a relationsship between a certain URL-route, and the function to be executed when the user enters accesses the URL.
The fuction decorated with ```@app.route``` should give instructions about what to display when the user visits a certain URL-route. What the function returns is returned to the browser as a response.


### Setting the flask environmental variable ###
(Assuming that the virtual environment have already been activated - see above for how-to)  
```set FLASK_APP = [PROJECT].py```

# Chapter 2 - templates #
This was an interesting learn for me, I had heard about tamplates and Jinja before, but I had assumed that it referred to ready-made templates.

### Mock objects ###
By creating mock-objects, we can focus on developing one specific part of the applicattion, without having to worry about the fact that certain parts aren't built yet. So, if I wanna create a profile-page, but I don't have a user-system yet, I can create a mock user object, and focus on the profile-page.

## Jinja 2 ##
Jinja2 is a templating engine

### Templates ###
By creating templates, it becomes easier to manage elements that are displayed across multiple pages, such as a menu bar. If we did not use templates, we would have to repeat the code for the menubar in the layout of each page. This both leads to repetition, both what is worse, it also means that if I wanna change something about the menubar, then I have to do it in everysimple page layout. By having a template instead, that I can apply to each page, I only need to make one change to the template, and the change will be reflected across all the pages where it occurs. 
We don't just use templates for repeated elements - all layout is contained in template files (.HTML files stored in the template folder).  

To display the layout defined in a template, we make the function return a ```render_template()``` call. The render_template function takes the name of the file containing the template as an argument, along with specification of values for any dynamic elements (see below). The replacements of placeholders during rendering is provided by the jinja2 engine.

### Dynamic elements / Placeholders ###
Ïn templates, there will sometimes be elements that we can't add ahead of run-time. A simple example would be if I had a page with the headline  
Hello [username]!
Where I would have the first name of the user who is logged in, take up the spot, where username is currently a placeholder. 
We use placeholders for elements of dynamic content, i.e. whatever is displayed there is not permanent or fixed.
In the .html code, we use ```{{   }}``` (double curly braces) around the placeholder.  

### Conditional statements ###
Conditional statements are given within curlybrace+%signs ```{%   %}```
Conditional statments can be used to give multiple possibilities for replacing a placeholder during rendering. E.g it can be used to give a default value, that we will revert to, if no value is passed with the render_template() call, which can fill the placeholder.

This is an example of the logic:
```html
{% if name %}        <!--If a variable was passed for name, use it-->
<!--HTML code for how to display the name, with a placeholder called {{ name }} where the value is filled in-->
{% else %}           <!--If no variable was passed for name, use this hardcoded default version instead-->
<!--HTML code with alternative, such as a hardcoded default-name. Contains no placeholders-->
{{% endif %}}
```

### Loops ###
Loops can be used for rendering an unknown number of elements. If we wanna display all blog-posts, but we don't know how many there are, because the number may increase (or decrease) from rendering to rendering, we can use loops.

This is an example of the logic (assuming that the render_template function is passinga list called posts
```html
{% for post in posts %}        <!--Looping over all elements in the passed list called posts-->
<!--Do with each post what is required-->
{% endfor %}
```

### Template Inheritance ###
If there are element that all templates on the pages should contain (such as footer, menus, nav-bars, etc.) this can be stored in a base template, that all other templates can inherit from. 

In the base template, ```block``` statements are used to show where content from inherited templates ccan be placed.
I think of the block-statement in the base template as a place holder where the corresponding block from an inheriting template will be filled in. 

Blocks are given unique names. That was, there can be multiple different places in the base where some code can be filled in, and the engine will know what piece of code to fill into which slot based on the name.

In the base template the block is given:
```html
{% block [name-of-block] %}
{% endblock %}
```

In the template that inherits from the base, the following is the structure:
```html
{% extends "[name-of-base-template-file].html" %}

{% block [name-of-block] %}
<--! The code to fill in this slot in the base template-->
{% endblock %}
{% block [name-of-block] %}
```

![templates](https://user-images.githubusercontent.com/32916783/41062951-1e3e38ea-698c-11e8-836e-c4e31deb08d5.png)

# Chapter 3 - templates #  

### Configurations ###
Because Flask and Flask extensions gives the user the possibility to determine certain things, those decisions needs to be specified somewhere. This could technically be done in the app file, but it is usually done in a config file on its own. 
The author suggests writing a config class in a seperate python file. 
In this chapter, we only have one thing that we need to configurate: the secret key (see below).
The configurations should to be applied to the app, so in the ```__init__.py``` file, we'll import it and create an instance of the class

```python
from config import Config #lower case config is name of file, uppercase config is name of class

...

app.config.from_object(Config)
```

### SECRET_KEY ###
The secret key is used in most flask applications. It is a cryptographic key, to protect against CSRF (Cross-site Request Forgery)
The secret key is used to generate signatures or tokens.

### Flask-WTF ###  
An extension for creating and handling forms
Using Flask-WTF we can create webforms. A webform is represented as a python class. Each field is defined as a class variable.
WTF-forms have many field-types. Each is a class, and when creating a field, it is an instantiation of that class, so it has to be called with arguments. (Below I will include the type of fields that the microblog is using so far - other field classes are documents [here](http://wtforms.readthedocs.io/en/latest/fields.html)

Because all the specific fieldtypes inherit from a field baseclass, they are all constructed with the following default parameters: (label=None, validators=None, filters=(), description='', id=None, default=None, widget=None, render_kw=None, \_form=None, \_name=None, \_prefix='', \_translations=None, \_meta=None).  
When we initialize any type of field we can construct it with the value we wish for these parameters, or we can just let it default. 
Usually we would at least want to set the label (the first argument, given as a string), and potentially also a validator for datareuqired, if we don't wish to let the user submit an empty field

```python
StringField #Field for Stringinputs - e.g. username, name, etc.
PasswordField #Field for password inputs
BoolenField #This is a checkbox, i.e., user can either leave it blank or fill it in. Can be used for remember-me at the end of the form
SubmitField #A button for submitting all the information filled into the form (unless a required field has not been field in yet), 
```

In order to be able to use any of these fieldypes, you will have to import them from wtforms
The fieldtypes are predefined with HTML rendering. 

_currently at form templates_
