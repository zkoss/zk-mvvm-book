# @CookieParam

Syntax
======

``` java
@CookieParam("keyString")
```

Description
===========

**Target:** A method's parameter (for initial and command methods)

**Purpose:** Tell binder to retrieve this parameter with specified key from the HTTP request cookie.

The annotation is applied to initial (or command) method's parameter. It declare the applied parameter should come from the HTTP request cookie with specified key.

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
