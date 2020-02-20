# @QueryParam

Syntax
======

``` java
@QueryParam

@QueryParam("keyString")
```

Description
===========

**Target:** A method's parameter (initial method and command method)

**Purpose:** Tell binder to retrieve this parameter with specified key from HTTP request parameters.

The annotation is applied to initial method's (or command method's) parameter. It declares that the applied parameter should come from HTTP request parameters with specified key.

> Since 9.0.1

The value can be omitted if name is the same as the annotated parameter.
```java
@QueryParam String keyString
```

Example
=======

Http request parameters is appended at URL like: 

`http://localhost:8080/zkbinddemo/httpparam.zul?param1=abc`

``` java
public class HttpParamVM {

    String queryParam;

    @Init
    public void init(@QueryParam("param1") String param1) {
        queryParam = param1;
    }
}
```

-   In this example, binder will pass "abc" to param1.
