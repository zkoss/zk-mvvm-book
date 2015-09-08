# @ImmutableElements
> Since 8.0.0

Syntax
======

``` java
@ImmutableElements
```

Description
===========

**Target:** collection

**Purpose:** Marker annotation to indicate that the elements of the collection for
 `FormProxyObject` are an immutable class.

The properties of an immutable class won't be tracked and thus reduce the resources needed in the application.

Example
=======

``` java
public class CustomVM {

    @ImmutableElements
    private Map<String, Object> info;

    // other code...
}
```

