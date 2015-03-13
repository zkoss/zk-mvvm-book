# @Default

Syntax
======

``` java
@Default("defaultValue")
```

Description
===========

**Target:** A method's parameter (for initial and command methods)

**Purpose:** Assign a binding parameter's default value when it's null.

You give annotation element's value with a String, and ZK will cast to the corresponding type of the parameter. You can apply this annotation after other parameter related annotation e.g., @BindingParam. If a parameter retrieved by first annotation is null, it uses the default value specified in this annotation.

Example
=======

#### Pass parameter from a zul
``` xml
<button id="first" onClick="@command('cmd', arg2=100)" />
<button id="second" onClick="@command('cmd')" />
```

#### Example to assign default value
``` java
    @Command
    public void cmd(@Default("false") Boolean arg1, @BindingParam("arg2") @Default("3") Integer arg2){
        //...
    }
```

-   According to above example, if we click first button, binder will pass 100 to arg2. If we click second button, arg2 will be 3.

