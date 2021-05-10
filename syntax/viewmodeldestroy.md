# @Destroy

> Since 8.5.2

Syntax
======

``` java
@Destroy

@Destroy(superclass=true)
```

Description
===========

**Target:** method, class

**Purpose:** Marker annotation to identify a destroy method.

Binder calls the method with this annotation when finalizing a ViewModel. In a ViewModel class, **only one destroy method is allowed at the most**. If you set annotation element **superclass** to **true**, the ViewModel's class's destroy method will be invoked first, then the parent's; this logic repeats on super class. If a class has no method with `@Destroy`, no method will be called (including the super class's)<sub>[1]</sub>.

For example, in a class hierarchy:
```java
class A {
	@Destroy
	public void destroyA() {}
}

class B extends A {
	@Destroy
	public void destroyB() {}
}

class C extends B {
	//no @Destroy method
}

class D extends C {
	@Destroy(superclass=true)
	public void destroyD() {}
}

class E extends D {
	@Destroy(superclass=true)
	public void destroyE() {}
}
```

E is the last child class.

- When binder finalizes **A**, it will...
	1. call A's `@Destroy` method `destroyA()`

- When binder finalizes **B**, it will...
	1. call B's `@Destroy` method `destroyB()`
	2. A's `@Destroy` method will **NOT** be called, because `superclass=true` was not set in B's `@Destroy`

- When binder finalizes **C**, it will...
	1. do nothing, because no method was annotated with `@Destroy`

- When binder finalizes **D**, it will...
	1. call D's `@Destroy` method `destroyD()`
	2. since `superclass=true`, binder will try to look for `@Destroy` method in C later
	3. no `@Destroy` method exists in C, so nothing will happen

- When binder finalizes **E**, it will...
	1. call E's `@Destroy` method `destroyE()`
	2. find `superclass=true`, binder will look for `@Destroy` method in D later
	3. call D's `@Destroy` method `destroyD()`
	4. find `superclass=true` again, binder will look for `@Destroy` method in C later
	5. no `@Destroy` method exists in C, so nothing happens

We can also use parameter related annotations on destroy method's parameters; please refer to subsections of [Syntax/ViewModel/Parameters](./parameters.html).

[1]: If you override parent class's destroy method and set `superclass=true`, due to Java's limitation, child's method will be called twice. To avoid this, you should **remove** `superclass=true` in child class and use `super` to invoke parent's implementation.
```java
class Parent {
	@Destroy
	public void destroyParent() {}
}

class WrongChild extends Parent {
	@Destroy(superclass=true) // binder will try to invoke parent's destroyParent()
	@Override
	public void destroyParent() {
		// but since parent's method was @Override by child
		// this method will be called twice
	}
}

class CorrectChild extends Parent {
	@Destroy // no superclass=true
	@Override
	public void destroyParent() {
		// child implementation of destroyParent()
		super.destroyParent(); // manually invoke super class's destroyParent() later
	}
}
```

Example
=======
``` java
public class FooViewModel {
    @Destroy
    public void destroyFoo() {
        //finalizing
    }
}

public class BarViewModel extends FooViewModel {
    @Destroy(superclass=true)
    public void destroyBar() {
        //destroy method of super class FooViewModel will be called later.
    }
}

@Destroy(superclass=true)
public class ChildViewModel extends BarViewModel {
	//method body
}
```
