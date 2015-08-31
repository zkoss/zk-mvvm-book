# @Init

Syntax
======

``` java
@Init

@Init(superclass=true)
```

Description
===========

**Target:** method, class (since 6.0.1)

**Purpose:** Marker annotation to identify a initial method.

Binder calls the method with this annotation when initializing a ViewModel. In a ViewModel class, **only one initial method is allowed at the most**. If you set annotation element **superclass** to **true**, the ViewModel's parent class's initial method will be invoked first then child's, and this logic repeats on super class. If a class has no method with @Init, no method will be called (including the super class's)<sub>[1]</sub>.

For example, in a class hierarchy:
```java
class A {
	@Init
	public void initA() {}
}

class B extends A {
	@Init
	public void initB() {}
}

class C extends B {
	//no @Init method
}

class D extends C {
	@Init(superclass=true)
	public void initD() {}
}

class E extends D {
	@Init(superclass=true)
	public void initE() {}
}
```

E is the last child class.

- When binder initializes **A**, it will...
	1. call A's `@Init` method `initA()`

- When binder initializes **B**, it will...
	1. call B's `@Init` method `initB()`
	2. A's `@Init` method will **NOT** be called, bacause `superclass=true` was not set in B's `@Init`

- When binder initializes **C**, it will...
	1. do nothing, bacause no method was annotated with `@Init`

- When binder initializes **D**, it will...
	1. since `superclass=true`, binder will try to look for `@Init` method in C first
	2. no `@Init` method exists in C, so nothing will happen
	3. call D's `@Init` method `initD()`

- When binder initializes **E**, it will...
	1. found `superclass=true`, binder will look for `@Init` method in D first
	2. found `superclass=true` again, binder will look for `@Init` method in C first
	3. no `@Init` method exists in C, so nothing happens
	4. call D's `@Init` method `initD()`
	5. call E's `@Init` method `initE()`

We also can use parameter related annotation on initial method's parameters, please refer to subsections of [Syntax/ViewModel/Parameters](./parameters.html).

[1]: If you override parent class's initial method and set `superclass=true`, due to Java's limitation, child's method will be called twice. To avoid this, you should **remove** `superclass=true` in child class and use `super` to invoke parent's implementation.
```java
class Parent {
	@Init
	public void initParent() {}
}

class WrongChild extends Parent {
	@Init(superclass=true) // binder will try to invoke parent's initParent()
	@Override
	public void initParent() {
		// but since parent's method was @Override by child
		// this method will be called twice
	}
}

class CorrectChild extends Parent {
	@Init // no superclass=true
	@Override
	public void initParent() {
		super.initParent(); // manually invoke super class's initParent()
		// child implementation of initParent()
	}
}
```

Example
=======
``` java
public class FooViewModel {
    @Init
    public void initFoo() {
        //initializing
    }
}

public class BarViewModel extends FooViewModel {
    @Init(superclass=true)
    public void initBar() {
        //initial method of super class FooViewModel will be called first.
    }
}

//since 6.0.1
@Init(superclass=true)
public class ChildViewModel extends BarViewModel {
	//method body
}
```
