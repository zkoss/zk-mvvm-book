# @Transient
> Since 8.0.0

Syntax
======

``` java
@Transient
```

Description
===========

**Target:** getter method

**Purpose:** mark an element transient, and the elemtent would not be cached when using a form
proxy.

Example
=======

``` java
public class VM {
    @Transient
    public Object getTotal() {
        return getFoo() * getbar();
    }
}
```
