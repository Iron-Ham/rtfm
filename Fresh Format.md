# Environment set up from a fresh install

## Install Xcode

 * The Xcode developer tools are essential for many of the dev tools we use to run on Mac OS X


    xcode-select install         <--- this will install command line dev tools

## Install atom.io

 * Install these packages:
1. linter
  * linter-jshint
  * linter-coffeelint
  * linter-pep8
  * linter-scss-lint
  * merge-conflicts

The linter will help you debug your code and highlight errors you might make  
merge-conflict helps to easily resolve merge issues

IF you get $PATH errors from atom you can modify your path settings for atom:
 * open your atom init script.. Go to Atom -> Open Init Script


    process.env['PATH'] ='/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin'

## Set up your terminal

1. Install oh-my-zsh [instruction](https://github.com/robbyrussell/oh-my-zsh)
2. Install [this](http://color.smyck.org) terminal theme  

We work a lot in the terminal, get used to it and start out right by setting up your environment

    You can modify your $PATH in ~/.zshrc

## Install Xamarin

Xamarin is a tool that allows developers to share business logic code amongst multi-platform apps.  
It uses the .NET framework and C#

## Install pip
pip is a tool used to install python packages such as django.  
[github](https://github.com/pypa/pip)

## Install virtualenvwrapper
[virtualenvwrapper](http://virtualenvwrapper.readthedocs.org/en/latest/install.html) is a wrapper for virtual environments and works in tandem with pip.  
instead of installing EVERY python package you use on the host computer, pip will instead run the packages in a virtual environment for you so that your computer remains fast and clean
 * you may need to update your .bashrc file to get the terminal commands to work

## Install Vagrant and VirtualBox
 * [vagrant](https://www.vagrantup.com) is a development environment. We use it in tandem with VirtualBox to run the database

 * [virtualbox](https://www.virtualbox.org) is will emulate a server running a database
