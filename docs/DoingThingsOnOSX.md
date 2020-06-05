[Back to main](readme.md)

[TOC]

# Doing everything locally on OSX
## Getting the environment set up
Most everything at the beginning is done via command line.  First step, create a new virtual environment if you hadn't already from a previous section.  These commands are for installing a specific python version, creating the virtual environment, creating a project folder and moving into it, and creating a file to tell pyenv which environment to use.  You can probably copy/paste these in one block into the shell as I'm relatively sure none would ask questions.  If a command in the block stops to ask a question, subsequent text in the pasted block will be used as that input.

    :::bash
    pyenv install 3.8.2
    pyenv virtualenv --copies 3.8.2 example-stocks
    mkcd ~/PersonalProjects/example-stocks
    echo "example-stocks" > .python-version

## Create Project Folders
This project includes a webserver, database, RESTful API, and workers.  Each needs their own folder so create them.  Rather than a command for each, we can use a fancy shell feature that I can rarely remember the name for.  The braces act as kind of a "do the command for each of these" loops.

    :::bash
    mkdir {web,db,worker,api}

Running `tree` will show the four folders created in the `example-stocks` folder.

    :::plainconsole
    .
    ├── api
    ├── db
    ├── web
    └── worker

## Initial Django Setup
Django is a web framework that makes building websites and APIs a bit easier.  It includes ways to handle database access such that you might not need to worry about writing SQL queries.

### Install Django
First, go into the web folder.

    :::bash
    cd web

We need to get Django installed into the virtual environment so we can get it to create the initial folder structure.  The custom is to have python requirements for a project in a plain text file called `requirements.txt`.  Use a text editor to add the content below (`vi requirements.txt`).  Technically, only the `Django` line is required but we will get around to using all of these.

    :::ini
    # Django
    Django
    
    # Utilities
    django-extensions
    Werkzeug
    
    # Settings / configuration
    django-configurations
    django-dotenv
    dj-database-url
    django-appconf
    psycopg2-binary
    
    # django-cache-url
    # python3-memcached
    
    # Certificate authorities
    certifi

Just for fun, check what is currently installed.

    :::bash
    pip list

There are only two packages currently installed.

    :::plainconsole
    Package    Version
    ---------- -------
    pip        20.1
    setuptools 41.2.0

Now, tell pip to install all of that crap.

    :::bash
    time pip install -r requirements.txt

There will be a pile of output but it shouldn't take too long.  Did you notice I snuck in a new command?  The `time` command will report back on how long the rest of the command took.

    :::plainconsole
    Collecting Django
      Using cached Django-3.0.6-py3-none-any.whl (7.5 MB)
    Collecting django-extensions
      Using cached django_extensions-2.2.9-py2.py3-none-any.whl (217 kB)
      
    ... snip ...
    
    Installing collected packages: sqlparse, pytz, asgiref, Django, six, django-extensions, Werkzeug, django-configurations, django-dotenv, dj-database-url, django-appconf, psycopg2-binary, certifi
    Successfully installed Django-3.0.6 Werkzeug-1.0.1 asgiref-3.2.7 certifi-2020.4.5.1 dj-database-url-0.5.0 django-appconf-1.0.4 django-configurations-2.2 django-dotenv-1.4.2 django-extensions-2.2.9 psycopg2-binary-2.8.5 pytz-2020.1 six-1.14.0 sqlparse-0.3.1
    
    real	0m18.660s
    user	0m7.584s
    sys	0m6.678s

The `real` line shows this took 18 seconds when I ran it.  Running `pip list` again shows more packages than what was listed in requirements.

    :::plainconsole
    Package               Version
    --------------------- ----------
    asgiref               3.2.7
    certifi               2020.4.5.1
    dj-database-url       0.5.0
    Django                3.0.6
    django-appconf        1.0.4
    django-configurations 2.2
    django-dotenv         1.4.2
    django-extensions     2.2.9
    pip                   20.1
    psycopg2-binary       2.8.5
    pytz                  2020.1
    setuptools            41.2.0
    six                   1.14.0
    sqlparse              0.3.1
    Werkzeug              1.0.1

The extra packages are dependencies from the packages that were listed in `requirements.txt`.

### Setting up the Django Project

Django has a command that sets up the folder structure and some files to bootstrap a simple project.

    :::bash
    django-admin startproject stockings .

After running, the `tree` output should look like:

    :::plainconsole
    .
    ├── stockings
    │   ├── __init__.py
    │   ├── asgi.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    ├── manage.py
    └── requirements.txt

No database has been configured so it will be pointing to an sqlite database.  Sqlite is a very basic database that is really just a file with some libraries that know how to manipulate it.  It is ok for just getting a project started but fails even with small work loads.

Stockings folder
:    The folder using the name you used on the `startproject` command is the main configuration folder for this project.  In my workplace, we renamed this as `config` but that requires some other file changes.

asgi.py and wsgi.py
:    These are for certain webserver apps that run more effeciently than the built-in runserver that Django has for local testing.  We won't worry about these just yet.

urls.py
:    This controls how Django decides which urls typed into the address bar lead to which app/view.

settings.py
:    This is where the project-level settings are grouped.  App-level settings will be handled by Django AppConf within each app.

## Configuring an IDE
Up to now, no IDE (integrated development environment) has been used.  I'd be surprised if most python IDEs didn't have some Django integration for most commands, but from what I've experienced, many of those integrations just kinda work.  When Django makes changes, they kinda break.  I've found it easier to do most things via command line.  Especially when things break, the error isn't hidden in IDE error messages.  That said, the IDE needs to know which virtual environment to use as it likely tracks which packages you are using and would constantly pop up warnings about missing requirements.

### Thonny
`Cmd+,` to open the preferences window.
Click the `Interpreter` tab and `Locate another python executable` box on that tab.
Brings up a `Finder` style window.
`Cmd+Shift+G` to bring up a prompt for a location as we need to get into a hidden folder.
Type `~/.pyenv/versions/` and press enter.  The `~` is a shortcut for the current user's home folder.
Navigate into whichever environment was created, then `bin` and select `python3`.
Open `Tools` and `Manage Packages`.  You should see a list on the left of about 15 or so packages.

#### Note About Thonny
I was unable to find a way for Thonny to deal with a multi-file project.  This will likely become an issue with large projects.  For a script thrown together to do a specific task, it should be ok.

### Pycharm
Start Pycharm and tell it to open the `web` folder.  Don't point it at the `example-stocks` folder as there will be another python project in the `api` folder.  It will take a moment to start as they've been adding more bloat to the app over time for new and happy features.
`Cmd+,` to open the preferences window.
Expand the `Project: web` tree and select `Project Interpreter`.
Click the gear icon at the top right and select `Add...`
Make sure `Virtual Environment` is selected on the left list and `Existing Environment` is the selected option on the right.
Click the `...` to the right of `Existing Interpreter` to bring up a new dialog box.
Type `~/.pyenv/versions/` and press enter.  The `~` is a shortcut for the current user's home folder.
Navigate into whichever environment was created, then `bin` and select `python3`.
It will take a moment to read through the packages and index the code for autocomplete goodness.  Once done, you should see a list of about 15 or so packages.

Pycharm likes to help you with your source code repository.  The pop up might show up in the bottom right corner to ask if you want to add project configuration files to the repo.  This is generally not a good idea.  First, if you are working in a team, each person could use a different application making chunks of the stored code useless.  If you are working alone, it would be ok except for the habit building it does to do so.  When you end up on a team, you might add the files just out of habit.  Pycharm's project settings are stored in a folder called `.idea` at the root of the project.

For more info on general Pycharm usage, search Youtube for it.  I might write some later if requested.  Just be aware there are Windows and Linux versions so videos might be for a different OS.  The visual layout of the IDE is very similar across OS' but the keyboard shortcuts are definitely different.

TODO: Is there more setup needed?  I think I skipped ahead and didn't write something down.

## Testing Django Without Any Additions
Now to see if everything installed correctly.  For Pycharm, click the green right arrow in the tool bar.  For the command line, make sure you are in the `web` folder and then type:

    :::bash
    ./manage.py runserver

Regarless of which you use, the output should be similar to the below.

    :::plainconsole
    Watching for file changes with StatReloader
    Performing system checks...

    System check identified no issues (0 silenced).

    You have 17 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
    Run 'python manage.py migrate' to apply them.
    June 05, 2020 - 16:14:12
    Django version 3.0.6, using settings 'stockings.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.

Note the `Starting development server at http://127.0.0.1:8000/` line.  This tells you what url to put into your browser.  In `terminal`, right-clicking the url should select the entire url and include an option to `Open URL`.  In `iTerm2`, holding `Cmd` will turn the url into a clickable link.  In Pycharm, the url is clickable without any additional action needed.  It should be obvious if it worked as the page should come up with a message including "The install worked successfully! Congratulations!"
If on the terminal or iTerm2, press `Ctrl+C` to stop it.  If in PyCharm, click the red square either on the top toolbar or to the left of the output panel.

## Creating a Hello World app
Of course, the first thing anyone should always do when learning a new language or framework is make it say 'Hello'.  To do this, 

Odd.  Cannot seem to have a code block immediately after a definition.

    :::bash
    manage.py showmigrations
    manage.py makemigrations
    manage.py migrate
    manage.py createsuperuser

[Back to main](readme.md)
