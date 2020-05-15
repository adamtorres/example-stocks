This doc shows examples of the formats that will be used.  This is not meant to be a complete list of markdown features.

## Detail/Summary - expandable sections

??? Moving the project to a local Docker
    Docker allows isolating components of a project (avoiding version and folder conflicts) but still has the potential port conflict issue as, in this case, docker is still running locally.  Using docker, you can more reliably reproduce an environment which helps when reproducing bugs.

??? Moving the project to a remote Docker
    The next step in the project's evolution is to go from running on the local system to running on a remote system.  This allows your dev machine to be less of a powerhouse as the heavy lifting is now done on some server elsewhere.  The drawbacks are a bit less freedom as you are likely sharing that server with other devs and some features do not work as expected.  Such as mounting the project's folder into the container such that changes made by the container happen to the code on the local system.  That can only easily work with a local docker.  It is possible to set it up to work remotely but that would require some folder sharing from the local system, mounting that share on the server, then mounting that mounted folder into the container.  But, if the work to be done is too much for the local system, then [moving to a remote docker]() is the next step.

???+ Testing
    This section should be open by default.  Testing is important, mkay.  Well written tests will tell you if a code change breaks something.  Test Driven Development (TDD) is when you write tests first, then write the code such that the tests pass.  As in, you know you need a function to strip the spaces out of a string.  So you write some tests which pass this unwritten function a variety of values and compare to expected results.  Then write the function and run the tests.  Fix issues, rerun tests, repeat until the tests pass.

??? Multiple paragraphs in one section
    Not sure if this will work but want to try.  The goal is to have multiple paragraphs be contained in the one collapsible section.  Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
    This is the second paragraph.  Ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

I wonder why the detail/summary thing doesn't work well in Textmate when there isn't anything after the item.

## Code blocks

    :::plaintext
    This is the 'plaintext' code block.
    It is meant for console output or whatever that doesn't need syntax highlighting.

    :::bash
    #! /usr/bin/env bash
    if [[ -z "$1" ]]; then
        echo "Missing arg."
        exit 9999
    fi
    echo "The arg is '$1'"

    :::python
    import time
    
    class Thing(object):
        def sound(arg):
            print(f"Beep goes the {arg}.")
    
    x = Thing()
    x.sound()

    :::sql
    SELECT 'thing' AS field, name
    FROM some_table
    ORDER BY name;

