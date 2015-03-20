# Basic Web Developer Workflow

This document describes the basic web development work flow for Relative Path, LLC

## Atom

 * Atom is the text editor that we use for web dev.
 * Atom has a linter with various language plugins to show errors in our code
 * Atom is fully integrated with Git
 * Atom is completely hackable to build any plugin we might need

Learn to use Atom and its features sooner rather than later


## Git
 Git is a version control program that allows multiple people to work on the
 same project simultaneously while storing the code in a central git repository

    git init                - creates a git repository in the current directory

    git status              - shows changes that haven't been added to the current commit

    git add --a             - adds all current changes to the current commit

    git commit -m "x"       - commits the current changes to your branch

    git push origin master  - pushes your current commits to the repository

    git pull origin master  - pulls any changes from the repository to your local computer


## Virtual Env Wrapper
Virtual Env Wrapper will install python packages into a VM on your computer so that they don't have to be installed into the main system. It reduces problems with conflicting packages.  

    mkvirtualenv name       - creates a virtual environment called "name"

    workon name             - switches your current virtual environment to "name"

    deactivate              - desctivates your current VE


## Pip
Pip is a python package installation tool. We use it all the time with python and django.

    pip install django      - installs the latest version of django into your current VE

    pip install *           - installs whatever package * is

    pip freeze              - shows a list of packages currently installed in the VE

    pip freeze > requirements.txt
    - write the output of 'pip freeze' to a file called 'requirements.txt'

    pip install -r requirements.txt
    - install all the python packages listed in 'requirements.txt'

## Vagrant
    vagrant up              - starts the vagrant environment
    vagrant destroy         - destroy the current box / system (deletes database)
    vagrant halt            - stops the box
