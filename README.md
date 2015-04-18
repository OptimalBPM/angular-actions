# Angular-actions
Angular Actions define a simplistic framework for spreading information. 
First within and later possibly between clients. A server implementation is outside the scope of this repository.

The functionality is meant to be an expanded "webbified" variant of the [Embarcadero Delphi TActionList](http://docwiki.embarcadero.com/Libraries/XE7/en/FMX.ActnList.TActionList) concept.

An action list is meant to centralize the responses to user actions. 
Action lists components maintain a list of actions that can be monitored by components in applications. 

For example, if a user, in a Role management interface, adds a new Role and then immidiately goes to the user management interface and wants to assign that role to a user, the role must appear in the list. 
If one implements it server-side, it could also be used to propagate changes and events across a system.

# Implementation

## Naming convention

An action can be called anything(like "sdlfkjsdlkfj"), but conventions are some times good, so here is an idea for how they should be named.
It could also serve to make the actions easier to filter, this is an namespace-inspired approach:
namespace/id-of-affected-thing/what-has-happened

### Crud actions


* item/1/create	
* item/1/remove	
* item/1/update	
* item/1/delete	

### Locking

* item/1/lock	
* item/1/release	
* item/1/releaseReq

### General events

* user/201/logout
* user/201/login
* user/201/update

And to select what actions to listen for, a component may then register using an xpath-like syntax:

* item	- Listen to all actions for all items
* item/1/* -	Listen to all actions for the item with id = 1
* item/1/update	 - Monitor the item with id = 1 for change
* user/\*/login | user/\*/logout - Monitor user login/logout activity
* item/1/release - Be told when an item is released for editing
* 


## Event structure
This should be kept simple:

    {action: string, data: object}

To post an event would then be simple:

    angularAction.post("item/1/create", data, global)
Where the scope would be set to global if one wanted to notify outside the local application(if the server implementation allows it).


# Security
Within a application, there are no real security issues, but if it was to be a system-wide implementation, these things would have to be considered:
* crud actions can only be generated by the server
* only actions regarding entities that the authenticate user have the right to be notified of should reach the client
* clients should only be able to post certain kinds of actions across the system 
