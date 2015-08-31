# @AfterCompose
> Since 6.0.2

Syntax
======
``` java
@AfterCompose

@AfterCompose(superclass=true)
```

Description
===========
**Target:** method, class

**Purpose:** Marker annotation to identify a life cycle method which will be invoked in doAfterCompose() of [BindComposer](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/BindComposer.html). **Only one @AfterCompose-annotated method is allowed at the most** in a ViewModel class. If you set annotation element **superclass** to **true**, the ViewModel's parent class's @AfterCompose-annotated method will be invoked first then child's, and this logic repeats on super class. If a class has no method with @AfterCompose, no method will be called (including the super class's).<sub>[1]</sub>.

For example, in a class hierarchy:
```java
class A {
	@AfterCompose
	public void doA() {}
}

class B extends A {
	@AfterCompose
	public void doB() {}
}

class C extends B {
	//no @AfterCompose method
}

class D extends C {
	@AfterCompose(superclass=true)
	public void doD() {}
}

class E extends D {
	@AfterCompose(superclass=true)
	public void doE() {}
}
```

E is the last child class.

- When [BindComposer](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/BindComposer.html) reaches **A**, it will...
	1. call A's `@AfterCompose` method `doA()`

- When BindComposer reaches **B**, it will...
	1. call B's `@AfterCompose` method `doB()`
	2. A's `@AfterCompose` method will **NOT** be called, bacause `superclass=true` was **NOT** set in B's `@AfterCompose`

- When BindComposer reaches **C**, it will...
	1. do nothing, bacause no method was annotated with `@AfterCompose`

- When BindComposer reaches **D**, it will...
	1. since `superclass=true`, BindComposer will try to look for `@AfterCompose` method in C first
	2. no `@AfterCompose` method exists in C, so nothing will happen
	3. call D's `@AfterCompose` method `doD()`

- When BindComposer reaches **E**, it will...
	1. found `superclass=true`, BindComposer will look for `@AfterCompose` method in D first
	2. found `superclass=true` again, BindComposer will look for `@AfterCompose` method in C first
	3. no `@AfterCompose` method exists in C, so nothing happens
	4. call D's `@AfterCompose` method `doD()`
	5. call E's `@AfterCompose` method `doE()`


We also can use parameter related annotation on AfterCompose method's parameters just as what we can do in @Init, please refer to subsections of [Syntax/ViewModel/Parameters](./parameters.html).

[1]: If you override parent class's @AfterCompose-annotated method and set `superclass=true`, due to Java's limitation, child's method will be called twice. To avoid this, you should **remove** `superclass=true` in child class and use `super` to invoke parent's implementation.
```java
class Parent {
	@AfterCompose
	public void doParent() {}
}

class WrongChild extends Parent {
	@AfterCompose(superclass=true) // BindComposer will try to invoke parent's doParent()
	@Override
	public void doParent() {
		// but since parent's method was @Override by child
		// this method will be called twice
	}
}

class CorrectChild extends Parent {
	@AfterCompose // no superclass=true
	@Override
	public void doParent() {
		super.doParent(); // manually invoke super class's doParent()
		// child implementation of doParent()
	}
}
```

Example
=======

``` java
public class FooViewModel {
    @AfterCompose
    public void doFoo() {
        //do while AfterCompose
    }
}

public class BarViewModel extends FooViewModel {
    @AfterCompose(superclass=true)
    public void afterComposeBar() {
        //AfterCompose method of super class FooViewModel will be called first.
    }
}

@AfterCompose(superclass=true)
public class ChildViewModel extends BarViewModel {
	// class body
}
```
