# @HeaderParam

Syntax
======

``` java
@HeaderParam("keyString")
```

Description
===========

**Target:** A method's parameter (applied on initial and command methods)

**Purpose:** Tell binder to retrieve this parameter with specified key from the HTTP request header.

The annotation is applied to the initial (or command) method's parameter. It declares that the applied parameter should come from HTTP request header with specified key.

Example
=======

``` java
public class HttpParamVM {

    String headerParam;

    @Init
    public void init(@HeaderParam("user-agent") String browser) {
        headerParam = browser;
    }
}
```
