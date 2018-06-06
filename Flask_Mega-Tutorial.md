[Tutorial here](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world)

# General Flask structure #
```
#Using [PROJECT] as the dummy-name of the project - in implementation you would replace this with an actual project-name  
[PROJECT]/
  venv/             #the virtual environment
  [PROJECT].py      #Main application module - The executed file
  app/              #app-folder
    __init__.py     #imports flask and make a flask instance?
    routes.py       #URL routes and their callback function
```

# Chapter 1 - Getting started#
## Virtual environment ##
Using a virtual environment means that you can use a specific version of something like flask for one project, while using another version for another project. This is because rather than making an installation for the entire machine, the installation is applied directly to the virtual environment. 
I wonder how this actually works. If I have a bunch of different virtual environment, do I have individual maps saved multiple times, taking up desk space? Or is it more like a git-type system, where the system just keeps track of which changes to apply to which “branch”, assuming that each virtual environment was equivalent to a branch?

```
#Activating a virtual environment    
(assuming it is already installed, and that I have navigated to the correct folder)  

venv\Scripts\active
```

## Decorators ##
It is kind of a weird name, decorators, becuase it kind of sounds like to me, that they are only there to make it look pretty, whichout actually playing any "real" role.  
But decorators modifies the function that immediately follow them.
Multiple decoraters can modify the same function. Decoraters can be used to create a call-back relationsship between some event and a function that execute on that event.  
The ```@app.route``` creates a relationsship between a certain URL-route, and the function to be executed when the user enters accesses the URL.
The fuction decorated with ```@app.route``` should give instructions about what to display when the user visits a certain URL-route. What the function returns is returned to the browser as a response.


## Setting the flask environmental variable ##
(Assuming that the virtual environment have already been activated - see above for how-to)  
```set FLASK_APP = [PROJECT].py```

# Chapter 2 - templates #
This was an interesting learn for me, I had heard about tamplates and Jinja before, but I had assumed that it referred to ready-made templates.

## Mock objects ##
By creating mock-objects, we can focus on developing one specific part of the applicattion, without having to worry about the fact that certain parts aren't built yet. So, if I wanna create a profile-page, but I don't have a user-system yet, I can create a mock user object, and focus on the profile-page.

## Templates ##
By creating templates, it becomes easier to manage elements that are displayed across multiple pages, such as a menu bar. If we did not use templates, we would have to repeat the code for the menubar in the layout of each page. This both leads to repetition, both what is worse, it also means that if I wanna change something about the menubar, then I have to do it in everysimple page layout. By having a template instead, that I can apply to each page, I only need to make one change to the template, and the change will be reflected across all the pages where it occurs. 
