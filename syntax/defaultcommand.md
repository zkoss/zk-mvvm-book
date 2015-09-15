# @DefaultCommand
> Since 6.5.1

Syntax
======
``` java
@DefaultCommand
```

Description
===========
**Target:** method

**Purpose:** To mark a default Command method.

When a binder receives a command, it starts to look for ViewModel's command methods by matching its name. If the binder cannot find a matched method, it invokes existing default command method.

Example
=======

#### Mark method as default
``` java
@DefaultCommand
public void defaultAction() {
    // method body
}
```
