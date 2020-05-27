[Back to main](readme.md)

[TOC]

# Django, Briefly
There is so much to Django that to have it all on a single page would be very unwieldy.  Only the basics for what will be used in the example project will be touched upon here.  For a more complete (and more accurate) documentation, see the [official docs](https://docs.djangoproject.com/en/3.0/).

## Django Terminology
Django is a framework to make it easier to translate url requests into understandable output while interacting with a database.

??? Views
    A view is a function which is called when a user tries to visit a page hosted by the server.  The function has access to a pile of information from the user's browser, the requested url, any submitted form fields, and possibly much more.  Using any or none of that, the function uses a template to build a response which is fed back to the user.  

??? Models
    Class is to Model is to Customer is to Adam
    Drawing is to Blueprint is to House is to 123 Main St
    A model is a description of an object of which you want to store information.  A model is a class that is designed to handle the various methods needed to store the information, allow its retrieval, modify the data, etc.  Django provides these classes to allow the programmer the ability to use a database without having to deal directly with the database.  Without them, the programmer would need to write their own SQL, the database access code to use that SQL, and modify all of that if they wanted to switch database engines.  With the framework, you use standardized commands all in python and the model framework does the SQL and database access.  Switching to a different database can be as simple as running migrations on the new database and exporting/importing the existing data.  Of course, in real life, there will always be surprises.

??? Migrations
    The migrations mentioned above are how Django tracks changes to a database.  For example, you create a model to represent a to-do list item.  It starts out with just a description and a completed flag.  When you first create it, Django creates an initial migration that will create the database table.  If you then add fields for created date and completed date, the database needs updated.  A new migration would be added to add those fields and to populate any existing records with some default value.  Over time, there might be a pile of migrations which might not be needed to be kept separate.  In the case of having customers for the project, keeping multiple versions around might be necessary.  If you know everyone is on a minimum version, you can squash migrations.  This compacts consecutive migrations into one so starting up a new database doesn't take as long.  Migrations can be as simple as letting Django handle it or you can create custom migrations to do specific things to the data.  The migrations are generally meant to be reversible in case of problems.  Hopefully, with testing, you shouldn't need to roll back any migrations.

??? Forms
    Forms are what define the input fields for views.  When you have some user interaction, usually with textboxes, buttons, sliders, etc, the form class defines those controls, any validation needed, and which model to use.

??? Mixin
    Snippets of classes that add features that are used more than once.  These help with the DRY principle - Don't Repeat Yourself.  An example would be a mixin for requiring login on certain views.  Adding the mixin to the view class would add that functionality with no additional effort.  Another example would be a mixin defining sets of fields commonly used in multiple forms along with the associated validation.  You could define separate form classes for the viewing, listing (multiple forms on one page), and editing without having to define all of the fields each time just by tacking on the mixin to the view classes.

[Back to main](readme.md)