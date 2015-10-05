# @Immutable

Syntax
======

``` java
@Immutable
```

Description
===========

**Target:** class, getter method

**Purpose:** Marker annotation to indicate an immutable class or a getter method.

The properties of an immutable class won't be tracked and thus reduce the resources needed in the application.

Once a getter method has been marked, the corresponding element would be cached non-recursively when using a form proxy.

Example
=======

``` java
@Immutable
public class SysConfiguration {
	// class body
}
```

``` java
public class DataObject {

    //other code...

    @Immutable
	public String getInfo() {
		return info;
	}
}
```
