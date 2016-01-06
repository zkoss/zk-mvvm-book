# @ImmutableFields
> Since 8.0.1

Syntax
======

``` java
@ImmutableFields
```

Description
===========

**Target:** class, getter method

**Purpose:** Marker annotation to indicate all of the fields returned from the given class are immutable (no setter).
Unlike [@Immutable](immutable.md), this annotation can be made as a proxy object for Form Binding, but not for all its fields.

Example
=======

``` java
public class VM {
    @ImmutableFields
    public Foo getFoo() {} // this Foo can make as a proxy, but not for its getDate() method.
}
public class Foo {
    public Date getDate() {} // this date object cannot make as a proxy.
}
```

``` java
public class VM {
    public Foo getFoo() {} // this Foo can make as a proxy, but not for its getDate() method.
}

@ImmutableFields
public class Foo {
    public Date getDate() {} // this date object cannot make as a proxy.
}
```
