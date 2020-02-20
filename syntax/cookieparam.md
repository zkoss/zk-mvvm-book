# @CookieParam

Syntax
======

``` java
@CookieParam

@CookieParam("keyString")
```

Description
===========

**Target:** A method's parameter (for initial and command methods)

**Purpose:** Tell binder to retrieve this parameter with specified key from the HTTP request cookie.

The annotation is applied to initial (or command) method's parameter. It declares that the applied parameter should come from the HTTP request cookie with specified key.

> Since 9.0.1

The value can be omitted if name is the same as the annotated parameter.
```java
@CookieParam String keyString
```

Example
=======

``` java
public class HttpParamVM {

    String cookieParam;

    @Init
    public void init(@CookieParam("nosuch") String guess) {
        cookieParam = guess;
    }
}
```
