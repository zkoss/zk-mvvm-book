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
 `FormProxyObject` are to be treated as immutable.

The properties of an immutable class won't be tracked and thus reduce the resources needed in the application.
Adding and removing items in the collection are tracked, but not changes to the items' fields.

Example
=======

``` java
public class CustomVM {

    @ImmutableElements
    private Map<String, Price> infos;

    // other code...
}
```

