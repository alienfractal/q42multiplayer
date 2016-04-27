# What's in the package? #

Thank you for your interest in the q42multiplayer engine. This documentation will help you understand the basisc of what you need to do in order to get your own multiuser application up and running.

You can start by either checking out the code from subversion, or download the zipfile.

# Requirements #

All you need is a machine that can host a .NET (2.0 or higher) web site, so any XP machine with IIS will do.

The code itself can be edited using your favorite editor, but it was written with Visual Studio in mind.

If you're new to Visual Studio, download the free [Visual Web Developer SP1](http://www.microsoft.com/express/vwd/). When using VWD, be aware that the solution file provided by the q42multiplayer zipfile (Demo.sln) requires Service Pack 1 of Visual Studio.

# After unzipping #

You see two folders:

  1. Demo (the Demo website, a simple chat application)
  1. Q42.Wheels.Multiplayer (the source code)

And the solution file (Demo.sln). Double click it to open up Visual Studio. You can run the demo website by simply pressing F5, or set up IIS to start the demo website as you see fit.

When you open a browser and surf to the demo website's url (usually http://localhost:SOMEPORT/Demo), you'll see the login page.

  1. Enter a name and press join.
  1. Open up a new browser tab and do the same.
  1. Chat with eachother.
  1. By typing "room anyname" you'll enter a newly created virtual room called "anyname", or any other name you specified. This demonstrates the use of rooms.
  1. To keep track of rooms and users, surf to Status.aspx.
  1. By closing a tab, you'll see the user disconnect.

# The demo code #

## Custom server implementation ##
The demo project has its own server implemented in the [app\_code/ChatServer.cs](http://code.google.com/p/q42multiplayer/source/browse/trunk/Demo/app_code/ChatServer.cs) file. Its implementation is pretty straightforward.

It implements the [Q42.Wheels.Multiplayer.Server](http://code.google.com/p/q42multiplayer/source/browse/trunk/Q42.Wheels.Multiplayer/src/Server.cs) abstract class. The one method that needs explaining is `ValidateProperty`. This is the single method that all user changes get by. If a user tries to set its name to "John", its arguments will be:

  * name = "name"
  * value = "John"

In the ValidateProperty method you can decide, for your application purposes:

  * If the give propertyName is allowed or not (if not, return null). So, this is the way to define what properties and events are used in your application.
  * If the value for the given propertyName is allowed. If not, return null. If you want its value to be altered (for profanity filtering for instance), this is where to implement it.
  * If the give property name/value pair is persistent or not. This is set by the last boolean parameter in the `new Property(name, value, true);` object that is returned. True is persistent, false is not.

### Creating the ChatServer instance ###
For this demonstration, a singleton instance of our little ChatServer is started when the website starts. This is done in [Global.asax](http://code.google.com/p/q42multiplayer/source/browse/trunk/Demo/Global.asax) using the `Application_Start` event.

To be notified of ending sessions, we call our own CharServer application's `OnSessionEnd` method in the `Session_End` event in Global.asax.

### Handling a user ping ###
Each user pings the server at a given interval, usually a second or so. Each ping is a call to a url, so for this example we have created ping.aspx, with its code behind [here](http://code.google.com/p/q42multiplayer/source/browse/trunk/Demo/ping.aspx.cs).

In the ping implementation, javascript is written to the client. For your own implementation, copy/paste the part that writes everything to the Response.

## The front end ##

You need to build your own html and javascript. For this demonstration we used a straightforward chat application. It requires a login, and then shows a chatlog and input field for you to type. The html is self explanatory and can be found in [default.aspx](http://code.google.com/p/q42multiplayer/source/browse/trunk/Demo/default.aspx).

Our chat app loads two javascript files.

  1. [Multiplayer.js](http://code.google.com/p/q42multiplayer/source/browse/trunk/Demo/js/Multiplayer.js) - The generic Multiplayer client library.
  1. [ChatClient.js](http://code.google.com/p/q42multiplayer/source/browse/trunk/Demo/js/ChatClient.js) - The application-specific logic.

Comments in the Multiplayer.js file are written with Visual Studio 2008 intellisense in mind, so you should get full intellisense thanks to the `/// <reference path="Multiplayer.js" />` line in the ChatClient.js file.

Upon joining, the ChatClient.join method calls Multiplayer.init, and tells it the ping url, the interval at which to ping, the event handler that takes care of business and what persistent properties and non-persistent events are possible.

The ChatClient.handleEvent method is the single point of interest where all user property-changes and event come in. This is where an application should take care of changes, keep track of new users, or those that disconnect or change rooms.

The rest should speak for itself.

Have fun!