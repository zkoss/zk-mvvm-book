# @ScopeParam

Syntax
======

``` java
@ScopeParam

@ScopeParam(scopes=Scope.APPLICATION)

@ScopeParam("keyString")

@ScopeParam(scopes=Scope.APPLICATION, value="keyString")
```

**A list of all scopes enumeration:**

``` java
enum Scope {
    //EXECUTION is since 8.6.0
    COMPONENT, SPACE, PAGE, DESKTOP, EXECUTION, SESSION, APPLICATION // single scope
    AUTO //find by comp.getAttribute(name, true)
}
```

Description
===========

**Target:** A method's parameter (for initial and command methods)

**Purpose:** Tell binder to retrieve a value with specified scope

The default scope: **AUTO** means searching the value from COMPONENT to SPACE, PAGE, DESKTOP, EXECUTION, SESSION, APPLICATION one by one automatically until a non-null value is found. If you specified the **scopes** element, binder will search the only scope you specified.

> Since 9.5.0

The value can be omitted if name is the same as the annotated parameter.
```java
@ScopeParam String keyString
@ScopeParam(scopes=Scope.SESSION) String keyString
```

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
