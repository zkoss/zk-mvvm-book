# @GlobalCommand

Syntax
======

``` java
@GlobalCommand

@GlobalCommand("commanName")

@GlobalCommand({"commanName1", "commandName2"})
```

Description
===========

**Target:** method

**Purpose:** To identify a global command method.

The optional annotation's element is a String for command's name and that name is referenced in a ZUL with global command binding. If it's not provided, method name is set as the command name by default.

We can use parameter related annotation on command method's parameters, please refer to subsections of [ Parameters](/syntax/parameters.html) for more information.

Example
=======

#### Method name as command name
``` java
    @GlobalCommand
    public void show(){
        //...
    }
```
#### Specify command name
``` java
    @Command("delete") @GlobalCommand("delete")
    public void deleteOrder(){
        //...
    }
```
