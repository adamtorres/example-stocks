[TOC]

# Yet Another Python Tutorial
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

[Shell of a Good Time](ShellOfAGoodTime.md)
[The Source of Control](TheSourceOfControl.md)
[Doing everything locally on OSX](DoingThingsOnOSX.md)