[TOC]

# Yet Another Python Tutorial
## Assumptions

Local system
:    You are using OSX as your local OS.  Some of the steps will be using `brew` to install packages.  Depending on how much of that there is, I might include steps for other systems.

Patience
:    You have an abundance of patience as I've not written a tutorial before.

Feedback
:    If you have questions, ask them.  If you have suggestions, make them.  This doc is a work in progress.

## Summary
This tutorial will cover setting up a webserver, database, RESTful API, and workers.  Initially, these will be set up on the local OSX system.  A subsequent section will show how to do the same on a local docker.  Then a remote docker system will be used while showing some benefits and limitations for doing so.

A quick overview of some differences running a project locally, on a local docker instance, or on a remote docker instance.

|feature|local pyenv|local docker|remote docker|
|----|----|----|----|
|code location|the code you edit is the code that runs|similar to pyenv, but the folder is mounted into the container|the code is copied so changes in the container are not made to the local copy|
|laptop power|needs to be powerful enough to run everything|everything + docker|laptop can just be a laptop.  Apps run on the remote system|
|ease of use|quick to python as no need for setting up docker configs|takes a bit to get configs right but they rarely change|takes longer as it includes managing a remote server|
|cleanliness|messy as even though pyenv keeps python bits separate, things like databases still install on local system.  Supporting libraries can also be an issue.  I've had problems with readline and openssl a couple of times|Cleaner than pyenv as all of the crap is contained in docker images and containers.  Remove those and the whole mess goes away.|Even cleaner as only the code is on the laptop.  The containers and images are on the remote server.|


***

# Document Style Guide
## Commands and Output
Commands and their output will be separated to limit confusion on what is a command and what is the output.  Both will be in monospace blocks to further differentiate them from the surrounding text.

The command to just display a line of text:

    :::bash
    echo "do something"

And the output:

    :::plainconsole
    do something

***

??? Command Line / Shell
    The command line is where much of the work can be done.  A graphical tool is not strictly necessary but tends to make things easier to understand.  The text-based nature of the shell is a limit for flashy animations and graphics but that could be seen as a benefit because it limits distractions.  If all you can see is the code, then there's less to get in the way of work.  For a [Shell of a Good Time](ShellOfAGoodTime.md), click the link.

??? Source Control / Repositories / Revision Control
    The above are common terms for the concept of keeping a history of changes to a code base.  Regardless of how they keep the history, it is a good thing as it allows developers to go back in time to revisit old versions. [The Source of Control](TheSourceOfControl.md) is knowing your project's history.

??? Creating the example project on the local system
    Running code locally is the most straight-forward route as far as understanding goes.  There are no containers, volumes, port translations, or any other garbage to get in the way.  If you start a database server that claims it is listening on port 5432, then the database server is listening on that port of your local system.  It potentially causes conflicts if you work on multiple projects where they might need different versions of software or want the same ports or the same folders.  Regardless of the pitfalls, the first step to this project will be [doing everything locally on OSX](DoingThingsOnOSX.md).

??? Moving the project to a local Docker
    Docker allows isolating components of a project (avoiding version and folder conflicts) but still has the potential port conflict issue as, in this case, docker is still running locally.  Using docker, you can more reliably reproduce an environment which helps when reproducing bugs.

??? Moving the project to a remote Docker
    The next step in the project's evolution is to go from running on the local system to running on a remote system.  This allows your dev machine to be less of a powerhouse as the heavy lifting is now done on some server elsewhere.  The drawbacks are a bit less freedom as you are likely sharing that server with other devs and some features do not work as expected.  Such as mounting the project's folder into the container such that changes made by the container happen to the code on the local system.  That can only easily work with a local docker.  It is possible to set it up to work remotely but that would require some folder sharing from the local system, mounting that share on the server, then mounting that mounted folder into the container.  But, if the work to be done is too much for the local system, then [moving to a remote docker]() is the next step.

??? Testing
    Testing is important, mkay.  Well written tests will tell you if a code change breaks something.  Test Driven Development (TDD) is when you write tests first, then write the code such that the tests pass.  As in, you know you need a function to strip the spaces out of a string.  So you write some tests which pass this unwritten function a variety of values and compare to expected results.  Then write the function and run the tests.  Fix issues, rerun tests, repeat until the tests pass.

??? Maintainability
    Be nice to the next guy.  It might very well be you coming back to some code in 6 months and having to read through everything to figure out how it was trying to work and why it is breaking.  Good names, separation of concerns, and short/to-the-point functions all help with that.

??? Self-updating containers?
    Not sure if this is a good idea.  It'd be something like the markdown-doc-server such that it clones or pulls on start so it gets the most updated code.  To make a running container do that would require some way for github to notify the container of changes.  Github allows for email notifications and webhooks.  Webhooks are where github POSTs to a specified url when certain actions happen.  A possibility would be to have a webhook service which is hosted at hookers.example.com.  This service would need to keep track of which url cooresponds to which docker container.  When it gets a hit on a given url, it either interacts with the container to tell it to gently pull new changes or it kills and restarts the container allowing the startup script to pull the new changes.  For the markdown-doc-server, the second would be the easiest as the existing container doesn't need changing and only the webhook service needs written.  This is not a good idea for anything that generates money as automatically putting new code into production is just begging for disaster.

Blarg