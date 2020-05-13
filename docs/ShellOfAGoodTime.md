[Back to main](readme.md)

[TOC]

# Shell of a good time
Before going into python, you need a way to get to it.  The shell is a quick way to get some things done without having to click around.  For some things, there's no other way to get it done.  The package manager "brew" has at least one option for a gui but it hasn't been updated for a couple of years.

## Shell basics
### Terminology

Folder and directory
:    Equivalent terms.  I use them interchangably.  when looking at file listings, they can be identified in a couple ways.  Using the below as an example, the far left column is for attributes and permissions.  The ones with a `d` are directories.  Also, because of the command used, the directory names are marked with a trailing `/`.  In this case, the folders are `.git`, `app`, and `docs`.  The two at the top, `.` and `..`, are special folders referring to the current folder and the parent folder, respectively.  I tend to ignore those as they are more symbolic.

    :::plaintext
    adam@Adams-MacBook-Air: ll
    total 72
    drwxr-xr-x  13 adam  staff   416B May 11 13:18 ./
    drwxr-xr-x   6 adam  staff   192B May 11 13:05 ../
    -rw-r--r--@  1 adam  staff   6.0K May 11 14:56 .DS_Store
    drwxr-xr-x  15 adam  staff   480B May 12 01:10 .git/
    -rw-r--r--   1 adam  staff   569B May 11 13:06 .gitignore
    -rw-r--r--@  1 adam  staff   296B May 10 23:49 Dockerfile
    -rw-r--r--@  1 adam  staff   181B May 11 12:25 Dockerfile.shortcut
    -rw-r--r--@  1 adam  staff   103B May 10 22:55 Dockerfile.shortcut.alpine
    drwxr-xr-x   4 adam  staff   128B May 11 23:41 app/
    -rwxr-xr-x@  1 adam  staff   324B May 11 13:15 d_build.sh
    -rwxr-xr-x@  1 adam  staff   359B May 11 13:18 d_run.sh
    -rwxr-xr-x@  1 adam  staff   172B May 10 23:56 d_shortcut.sh
    drwxr-xr-x   3 adam  staff    96B May 11 13:32 docs/

Shell, Command line, CLI
:    Commonly used interchangably, these refer to the text-based interface.  Technically, I believe `shell` is really the application which provides a command line interface (CLI).  Common shells are `bash` and `sh`.  OSX uses a slightly customized bash shell.  Googling help for a command by "bash [command]" is a good starting point.  If that doesn't help, try adding "OSX" as there are some quirks to how Apple did things.



### Comments from the peanut gallery
Any line starting with `#` will be ignored.  There have been times I've started a long command but didn't want to run it just yet.  To "save" it, I'd go to the start of the line and add a `#` and press enter.  It would save the line in the command history but not actually run it.  I could then press up arrow and remove the `#` to run the command.

### Quick text editors
This is a sensitive subject, see [the wiki on it](https://en.wikipedia.org/wiki/Editor_war).  The editors I've used for a long time is `vi` and `vim`.  The latter is an improved version of the former.  It takes a bit of getting used to but is quick and hasn't crapped out on any of the small files I've given it.  Only on files of a size that hurt most text editors does it start to fail.  I only use the basic text-editing functions (moving around and find/replace) but it has a lot of other functionality.  The main reason to use some form of text editor in the shell is speed.  If you're in a folder on the shell, typing `vi somefile.txt`, is a lot faster than switching to Finder, navigating to the folder, and double-clicking the file.  Or, if the default action is to execute as with shell scripts, right-clicking the file, selecting "Open with...", "Other", then finding the app to use.

### Terminal Condition
The terminal application used doesn't matter too much if you're just poking around.  If you use the shell much more than that, quirks of the application can start to make a big difference between "just popping in to poke at something" and "holy crap, I hate doing this."  The default terminal application in OSX made me seek out an alternative so long ago, I'm not certain exactly what quirks there were.  The terminal application I use is called [iTerm](https://www.iterm2.com/).  It isn't in the App Store for whatever reason, so you'd have to go to their site to download.

### Amazing Set of Pipes
A very useful feature of the shell is the ability to chain commands.  The output of one command becomes the input of another.  I've yet to reach a technological limit of chaining commands in the time I've spent working.  I've definitely hit an understanding limit, though.  The more commands in the chain, the more difficult it is to find and fix errors.  Commands are chained together with the pipe (`|`) character.  One extremely useful example of this is by using `grep`.  Grep searches its input for whatever you tell it to.  The example below shows a very basic way to find any line of output from the first command which contains the "wanted text".

    :::bash
    some-command-with-a-lot-of-output | grep "wanted text"

If you just want to search a text file, `grep "wanted text" filename` would work without having to pipe.

***

## Setting up a shell session
When starting a terminal, a few scripts are executed to set up the session.  For `bash`, the shell used by OSX by default, one of those files is `.profile`.  The leading `.` makes the file hidden from normal listings unless expressly requested.  If you look at your home folder in Finder, you will likely not see many files.  There are many files and folders in your home folder that are hidden by the leading `.`.  There is nothing special about a hidden file or folder other than the OS not showing it unless explicitly asked as in the command `ls -a`.  The `ls` command lists contents of a folder.  The `-a` flag tells `ls` to include hidden files/folders.
The `.profile` script is where you can put customizations to make moving around the shell easier.  Things like configuring what the shell prompt includes, alias commands, initialization commands for some installed components, etc.  This script isn't any different than any other script you can create other than the OS runs it whenever a new shell session is started.  You could add `echo "Hello There."` to the script and it would happily run that command every time a new shell is started.


### The Shell Prompt
The shell prompt is what the shell displays when it is waiting for a command.  This is my shell prompt starting in my home folder (`~`) and then moving to a Projects folder.

    :::plainconsole
    16:06:32 ~
    adam@Adams-MacBook-Air: cd Projects/
    16:06:40 ~/Projects
    adam@Adams-MacBook-Air:

My prompt includes the time when the prompt was displayed and the current folder on one line and then the user@host on the second line.  I'm not entirely sure what does this, but when I'm inside a pyenv-controlled folder, the prompt includes the current environment name.  My guess is, this is something pyenv does automagically.

    :::plainconsole
    (example-stocks) 16:08:37 ~/PersonalProjects/example-stocks
    adam@Adams-MacBook-Air:

In the `.profile`, you can add these two lines to set the prompt.  Check for any existing lines that set `PS1` and comment them out - put a `#` in front of the line.

    :::bash
    # (\t)ime (\w)orking directory (\n)ewline (\u)ser@(\h)ost
    export PS1="\t \w\n\u@\h: "

### Alias for long commands
An alias in the shell is simply a way to make an often-used long command into a short-to-type command.  In this case, `wigo` stands for "What is going on?" and was initially created by one of my coworkers years ago.  It isn't complex but shows a couple things just as a sanity check to make sure you
have everything where it should be and all is working fine.  The two commands are checking the python version and then showing how pyenv set that version.
To set the alias:

    :::bash
    alias ll="ls -laph"

This makes it so typing `ll` will be as if you typed `ls -laph`.  This is especially useful for oft-typed commands.  In this case, it lists all the contents of a folder using the long format (`-l`), including hidden files (`-a`), marks which ones are a folder (`-p`) and use human-readable sizes (`-h`).


    :::bash
    # brew install pyenv
    export PYENV_ROOT="$HOME/.pyenv"
    export PATH="$PYENV_ROOT/bin:$PATH"
    if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi

### Some things in my profile I'm not sure where they came from
These are for a package meant for pyenv which claims to manage virtual environments.  Not sure if I explicitly installed it or if it came with something.  Odds are, I installed it manually as the comment in this block is the command to do so.

    :::bash
    # brew install pyenv-virtualenv
    if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi

***

## General Helpful Commands
These are helpful things that are set up in the `.profile`.  Commands that are useful but don't need any setup will be described later.

### Listing files
These two show file listings.  I mainly use `ll` but `lt` adds on a sort such that the most recent files are at the bottom - right where you'd see them even if the rest scroll off the top of the screen.  The comment briefly describes the options used in the two commands.

    :::bash
    # (l)ong, (a)ll, (p)ath marked, (h)uman numbers, (t)ime sorted, (r)everse sort
    alias ll='ls -laph'
    alias lt='ls -laphtr'

"Human numbers" are "K" for kilobytes, "B" for Bytes, etc.
"Path marked" means a "/" is shown at the end of a name to show it is a folder instead of a file.
"All" means also show hidden/special files.
"Long" includes details of each file/folder such as permissions, owner, size, and modified date.

### Listing USB devices
Playing with external drives and other such USB devices generally "just works" but for those times when you need a little more info, this can help.  There's a command in linux called `lsusb` which lists detiails of all USB devices.  As the comment claims, this isn't exactly equivalent.

    :::bash
    # Not exactly the same as lsusb but does what I need when I think of lsusb.
    # That is, list the usb devices.
    alias lsusb='system_profiler SPUSBDataType'

### Creating folders
A common action at the command line is to create a folder and move into it.  It is so common, this alias gets decent usage.

    :::bash
    # Combines mkdir and cd commands.
    function mkcd () { mkdir -p "$@" && eval cd "\"\$$#\""; }

### Python Sanity Check
When working on many projects that are configured via pyenv, things can get a bit confusing.  Generally, when you get a lot of 'not installed' errors, you might have the wrong environment set up.  This little alias will tell you which version of python is being used and from which virtual environment.

    :::bash
    # Tell me What Is Going On
    alias wigo='python -V; pyenv version'

Now, when you type `wigo`, it should show something like:

    :::plainconsole
    Python 3.8.2
    example-stocks (set by /Users/adam/PersonalProjects/example-stocks/.python-version)

Stepping back to a folder that doesn't have an explicitly set version will show something like:

    :::plainconsole
    Python 3.7.3
    3.7.3 (set by /Users/adam/.pyenv/version)

### List Open Ports
Having multiple services running is all well and good.  Having them continue to run when you told them to stop isn't quite as good.  This command will show which ports are listening on your local system.  Only useful for when developing directly on OSX and marginally useful when using a local docker.  The long list of `-e ^Words` is for excluding things that don't matter.  Since the goal of this command is to show things that are related to python projects, ports for Chrome, Firefox, and Slack didn't need to be shown.

    :::bash
    # List open network connections while hiding the ones from boring applications and such we likely don't care about.
    alias op='lsof -n -i -P | grep -v -e ^Microsoft -e ^Dropbox -e ^GitHub -e ^Google -e ^Finder -e ^Office365 -e ^firefox -e ^sharingd -e ^SystemUIS -e UserEvent -e ^ARDAgent -e ^Slack -e ^WiFi -e ^com\.apple'

### Cleanup on aisle 9
Python is a scripting language.  For this example, that means you do not have to compile the code and then run the executable.  The python interpreter does that for you behind the scenes.  The files with `pyc` extensions are a compiled form and are created when needed.  The `__pycache__` folder is some folder for cache (I don't recall and am too lazy to google it).  The last one, `*.*.orig`, is what I would do to files when playing with things.  I'd rename the original file by appending `.orig` to the filename and put a modified version in place.  This seems a tad dangerous if you forget to put the original back.  But then, you are using source control, right?

    :::bash
    # TODO: Does this need findutils installed?
    # brew install findutils
    # Removes cached python files
    alias clean='find . -name *.pyc -delete && find . -type d -name __pycache__ -delete && find . -name *.*.orig -delete'

### Make Like a Tree
This slightly bridges the gap between a pure text shell and Finder.  This will show a text-based directory layout with indentation and such.  The issue is, I often need to see hidden files but not all hidden files.  Source control repositories add `.git` or `.hg` folders to a project and each have a ton of files.  Pycharm adds a `.idea` folder which also has a ton of files.  My solution was to create a few alias' which ignore certain folders.  The `bush` is a short tree in that it only goes 2 levels deep, `-L 2`.  The `realtree` is for when you need to see all of the files without a filter.

    :::bash
    # brew install tree
    # A sorta short version of tree which only shows 2 levels of folders
    alias bush='tree -d -L 2 -I "__pycache__|.hg|.git|.DS_Store|htmlcov|.hgcheck|src"'
    alias tree='tree -a -I "__pycache__|.hg|.git|.DS_Store|htmlcov|.hgcheck|src|.idea" --dirsfirst'
    alias realtree='/usr/local/bin/tree'

***

## Getting Python Set Up for a Project
This covers getting a specific version of python installed and creating a virtual environment to isolate a new project from any existing projects.

### Install pyenv
pyenv is a nice tool to control multiple versions of python and multiple environments of those versions.  Multiple environments allows each project to have its own world and reduces the chance of version conflicts.  There still is a chance that a module you want to use has a requirement conflict with some other module you already use.

    :::bash
    brew install pyenv

Brew always updates itself whenever it is run (possibly only on certain commands).  You will normally see sections titled:

    :::plainconsole
    ==> Auto-updated Homebrew!
    ==> New Formulae
    ==> Updated Formulae
    ==> Updated Casks

If pyenv is already installed, the last couple of lines will look like:

    :::plainconsole
    Warning: pyenv 1.2.18 is already installed and up-to-date
    To reinstall 1.2.18, run `brew reinstall pyenv`

### Install the wanted python version
Make sure the python version you want is installed.
The output shows all installed versions and created environments.  The one with the asterisk is the current one and the parens tell how it was set. In this case, 3.7.3 is set by my global setting.  Any folder that doesn't somehow set the version will use 3.7.3.

    :::bash
    pyenv versions

Example output:

    :::plainconsole
      system
    * 3.7.3 (set by /Users/adam/.pyenv/version)
      3.8.2

If you do not see the version you want, like 1.2.3, you can install it by verifying that version is available to pyenv and then installing it.  First, check if it is available.  This is a long list so you will have to scroll.

    :::bash
    pyenv install --list

Then, install the version.  For example, 2.7.17 is not on my system, so...

    :::bash
    pyenv install 2.7.17

But that failed with

    :::plainconsole
    BUILD FAILED (OS X 10.15.4 using python-build 20180424)
    ...
    checking whether the C compiler works... no
    configure: error: in `/var/folders/rq/l0m3j_554xq0mcd5b728l_bc0000gn/T/python-build.20200508103332.39819/Python-2.7.17`:
    configure: error: C compiler cannot create executables

Googled a bit and found it might be an xcode thing.  Since I had just upgraded to Catalina, this might've been reset.  Running this takes a bit of time.  It opens a window to continue the install.  Eventually shows a 'done' button.

    :::bash
    xcode-select --install

Retry the installation of 2.7.17.  Seems to be working.  Made my poor Air's fan run for a bit.

    :::bash
    pyenv install 2.7.17

It printed the below over a short time and ran for a bit without any further output.

    :::plainconsole
    python-build: use openssl from homebrew
    python-build: use readline from homebrew
    Installing Python-2.7.17...
    python-build: use readline from homebrew
    python-build: use zlib from xcode sdk

Then, after a few minutes and fan running, it completed.

    :::plainconsole
    Installed Python-2.7.17 to /Users/adam/.pyenv/versions/2.7.17

Now, checking versions available includes 2.7.17.
But we don't want that one.  It was just for example.  Get rid of it.

    :::bash
    pyenv uninstall 2.7.17

Example output (with a "y" as answer to the question):

    :::plainconsole
    pyenv: remove /Users/adam/.pyenv/versions/2.7.17? y

### Create a virtual environment for this project.
At the time of writing, 3.8.2 is the most recent non-alpha/beta release.

    :::bash
    pyenv virtualenv --copies 3.8.2 example-stocks

Breakdown:

virtualenv
:    tells pyenv you want to create a virtual environment

--copies
:    means to copy global packages instead of inherit.  Protection against someone/thing installing something at the global level which breaks this environment.

3.8.2
:    the version to install.

example-stocks
:    The name for the new environment.

Example output:

    :::plainconsole
    Looking in links: /var/folders/rq/l0m3j_554xq0mcd5b728l_bc0000gn/T/tmpbu5133f3
    Requirement already satisfied: setuptools in /Users/adam/.pyenv/versions/3.8.2/envs/example-stocks/lib/python3.8/site-packages (41.2.0)
    Requirement already satisfied: pip in /Users/adam/.pyenv/versions/3.8.2/envs/example-stocks/lib/python3.8/site-packages (19.2.3)

This creates the environment but doesn't mark any folders to use it.  Go to wherever you create projects.  For me, it is ~/PersonalProjects.

    :::bash
    cd ~/PersonalProjects

Create the root folder for this example project.  The name does not have to be the same as the virtual environment.  The command used here is from the "General Helpful Commands" section.  It is just a combination of `mkdir` and `cd` to make and change into a folder.

    :::bash
    mkcd example-stocks

Create the file that tells pyenv to use the new environment for this folder and any subfolders.

    :::bash
    echo "example-stocks" > .python-version

Now, when you check pyenv versions, it should look more like this:

    :::plainconsole
      system
      3.7.3 (set by /Users/adam/.pyenv/version)
      3.8.2
    * example-stocks (set by /Users/adam/PersonalProjects/example-stocks/.python-version)

Check using the `wigo` command from way up above.  The output should be similar to:

    :::plainconsole
    Python 3.8.2
    example-stocks (set by /Users/adam/PersonalProjects/example-stocks/.python-version)

Setting up a virtual environment doesn't seem to pick up the most recent `pip` package.  Since I've gotten warnings about it many times, the first thing I do is upgrade it.  Pick one of the following.  I tend to go with the fully spelled out word for random reasons.

    :::bash
    pip install pip --upgrade
    pip install -U pip
    # Note: the "-U" is capitalized.

Expected output should be similar to the below if pip does get upgraded.

    :::plainconsole
    Collecting pip
      Using cached https://files.pythonhosted.org/packages/54/2e/df11ea7e23e7e761d484ed3740285a34e38548cf2bad2bed3dd5768ec8b9/pip-20.1-py2.py3-none-any.whl
    Installing collected packages: pip
      Found existing installation: pip 19.2.3
        Uninstalling pip-19.2.3:
          Successfully uninstalled pip-19.2.3
    Successfully installed pip-20.1

Or this, if pip is already up-to-date.

    :::plainconsole
    Requirement already up-to-date: pip in /Users/adam/.pyenv/versions/3.8.2/envs/example-stocks/lib/python3.8/site-packages (20.1)

At this point, you have a clean virtual environment ready for a new project.

[Back to main](readme.md)
