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

**Purpose:** mark an getter transient, and the field will be ignored by the proxy handler.
This is useful to implement calculated fields (often readonly) that depend on other getters (which may be cached in a form proxy).

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
