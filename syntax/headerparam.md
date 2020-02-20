# @HeaderParam

Syntax
======

``` java
@HeaderParam

@HeaderParam("keyString")
```

Description
===========

**Target:** A method's parameter (applied on initial and command methods)

**Purpose:** Tell binder to retrieve this parameter with specified key from the HTTP request header.

The annotation is applied to the initial (or command) method's parameter. It declares that the applied parameter should come from HTTP request header with specified key.

> Since 9.0.1

The value can be omitted if name is the same as the annotated parameter.
```java
@HeaderParam String keyString
```

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
