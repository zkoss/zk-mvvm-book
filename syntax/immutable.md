# @Immutable

Syntax
======

``` java
@Immutable
```

Description
===========

**Target:** method, class

**Purpose:** Marker annotation to indicate an immutable class or a method.

The properties of an immutable class won't be tracked and thus reduce the resources needed in the application.

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
