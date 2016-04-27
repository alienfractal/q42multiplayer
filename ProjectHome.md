**[Q42](http://q42.nl)'s Multiplayer engine** is a lightweight, generic multi-user solution, allowing developers to create their own browserbased application or game over port 80.

The included sample chat implementation (written for demonstration purposes) uses less than 100 lines of server and client code combined.

A live demo can be found [here](http://multiplayer.q42.nl).

Each user asynchronously pings a url on the central server at a given interval, sending its changes and retrieving changes of other visitors. The retrieved changes are dispatched through the javascript library, so the clientside application may act upon it.

The client consists of a single 5k javascript file, and the server is a single C# class library that can be embedded as source, or as a dll.

Focus of the q42 multiplayer engine was to give developers full control and not come up with any restrictions to the type of application that is being developed.

## Features ##
  * Easy customization and full control. Example project included
  * Easy grouping of users that can see eachother by means of virtual "rooms"
  * Automatic room upscaling when a certain amount of users is reached
  * Automatic cleanup of empty rooms and idle users
  * Persistent user properties (for example: name, x or y position)
  * Nonpersistent user events, such as chat messages
  * Full control over rooms, names, properties, events and its values
  * Moderating features can be implemented easily
  * Optionally allowing multiple users per session, so each browser-tab can represent a new user.