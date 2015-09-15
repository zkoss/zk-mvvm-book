# @DefaultGlobalCommand
> Since 6.5.1

Syntax
======

``` java
@DefaultGlobalCommand
```

Description
===========

**Target:** method

**Purpose:** To mark a method as a default global command.

When a binder receives a global command, it starts to look for ViewModel's global command methods by matching its name. If the binder cannot find a matched method, it invokes existing default global command method.

Example
=======

#### Mark a default global command method
``` java
@DefaultGlobalCommand
public void defaultAction() {
    // method body
}
```
