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

For example, in a class hierarchy: A(has @init) &lt;- B(has @init) &lt;- C(no @init) &lt;- D (has @init, superclass=true). D is the last child class.

-   When binder initializes D as a view model, it will call D's initial method.
-   When binder initializes C, no method will be called.
-   When binder initializes B , it will call As' then B's.
-   When binder initializes A, it will call A's.

We also can use parameter related annotation on initial method's parameters, please refer to subsections of [Syntax/ViewModel/Parameters](/syntax/parameters.html).

[1]: If you override parent class's initial method e.g. Parent.m1() &lt;- Child.m1(). Because of Java's limitation, binder still call Child.m1(), and Child.m1() will be called twice. To avoid this, you should set superclass to false of Child.m1() and call super.m1() inside it.

Example
=======
``` java
public class FooViewModel{
    @Init
    public void initFoo(){
        //initializing
    }
}

public class BarViewModel extends FooViewModel{
    @Init(superclass=true)
    public void initBar(){
        //initial method of super class FooViewModel will be called first.
    }
}

//since 6.0.1
@Init(superclass=true)
public class ChildViewModel extends BarViewModel{

}
```
