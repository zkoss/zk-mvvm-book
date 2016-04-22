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

**Purpose:** Marker annotation to indicate all of the fields on this object (or objects of this class) are treated as immutable.
Unlike [@Immutable](immutable.md), this annotation still enables proxy createion for the current object in a Form Binding, but no proxies are created for any of its fields.

Example
=======

``` java
public class VM {
    @ImmutableFields
    public Foo getFoo() {} // this Foo will still be proxied, but not its date field.
}
public class Foo {
    public Date getDate() {} // no proxy created for the returned date object.
}
```

``` java
public class VM {
    public Foo getFoo() {} // this Foo will still be proxied, but not its date field.
}

@ImmutableFields
public class Foo {
    public Date getDate() {} // no proxy created for the returned date object.
}
```
