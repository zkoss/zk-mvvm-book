# @QueryParam

Syntax
======

``` java
@QueryParam("keyString")
```

Description
===========

**Target:** A method's parameter (initial method and command method)

**Purpose:** Tell binder to retrieve this parameter with specified key from HTTP request parameters.

The annotation is applied to initial method's (or command method's) parameter. It declares that the applied parameter should come from HTTP request parameters with specified key.

Example
=======

Http request parameters is appended at URL like: 

`http://localhost:8080/zkbinddemo/httpparam.zul?param1=abc`

``` java
public class HttpParamVM {

    String queryParam;

    @Init
    public void init(@QueryParam("param1") String parm1) {
        queryParam = parm1;
    }
}
```

-   In this example, binder will pass ¡§abc¡¨ to parm1.
