# @Immutable

Syntax
======

``` java
@Immutable
```

Description
===========

**Target:** class, getter method

**Purpose:** Marker annotation to indicate a class or result of a getter is to be treated immutable. As a result no Proxy is created around the object.

Once a getter method has been marked, the object itself will be cached non-recursively when using a form proxy.
Calling setters on such an object directly invoke the original setter and thus won't be tracked. 

also see: [ProxyHelper.addIgnoredProxyClass(java.lang.Class)](https://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/proxy/ProxyHelper.html#addIgnoredProxyClass%28java.lang.Class%29)
This also reduces the resources needed in the application. 

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
	public Price getPrice() {
		return price;
	}
}
```
