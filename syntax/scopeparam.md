# @ScopeParam

Syntax
======

``` java
@ScopeParam("keyString")

@ScopeParam(scopes=Scope.APPLICATION, value="keyString")
```

**A list of all scopes enumeration:**

``` java
enum Scope {
    COMPONENT, SPACE, PAGE, DESKTOP, SESSION, APPLICATION, // single scope
    AUTO //find by comp.getAttribute(name, true)
}
```

Description
===========

**Target:** A method's parameter (for initial and command methods)

**Purpose:** Tell binder to retrieve a value with specified scope

The default scope: **AUTO** means searching the value from COMPONENT to SPACE, PAGE, DESKTOP, SESSION, APPLICATION one by one automatically until a non-null value is found. If you specified the **scopes** element, binder will search the only scope you specified.

Example
=======

``` java
public class ScopeParamVM {

    @Init
    public void init(
    	@ScopeParam(scopes=Scope.APPLICATION , value="config") String sysConfig,
        @ScopeParam(scopes=Scope.SESSION,value="user") String userCredential) {
        // method body
    }
}
```
