# How to start a web server easy mode OSX

First familiarize yourself with apache. You might soon realize that it comes bundled with OSX

    sudo apachectl start
This will start the apache server


In your browser go to

    127.0.0.1
You should get a message that says "It works!"

The page being displayed is inside the default folder for such things
* /Library/WebServer/Documents

Now instead of altering apache's config file to show our own site, I find it easier to create a symbolic link in the default folder's file-tree.

    sudo ln -s ~/PathTo/MySite/ /Library/WebServer/Documents/MySite
now go to

    127.0.0.1/MySite/


when your done remember to shut down your apache server with the stop command

to remove the symbolic link you simply delete it like any other file
