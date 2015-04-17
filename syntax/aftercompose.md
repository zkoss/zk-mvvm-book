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

For example, in a class hierarchy: A(has @AfterCompose) &lt;- B(has @AfterCompose) &lt;- C(no @AfterCompose) &lt;- D (has @AfterCompose, superclass=true). D is the last child class.

-   When [BindComposer](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/BindComposer.html) start to handle D while in doAfterCompose(), it will call D's @AfterCompose method.
-   When [BindComposer](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/BindComposer.html) reaches C, no method will be called.
-   When [BindComposer](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/BindComposer.html) reaches B , it will call As' then B's.
-   When [BindComposer](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/BindComposer.html) reaches A, it will call A's.

We also can use parameter related annotation on AfterCompose method's parameters just as what we can do in @Init, please refer to subsections of [Syntax/ViewModel/Parameters](./parameters.html).

[1] If you override parent class's @AfterCompose-annotated method e.g. Parent.m1() &lt;- Child.m1(). Because of Java's limitation, [BindComposer](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/BindComposer.html) still call Child.m1(), and Child.m1() will be called twice. To avoid this, you should set `superclass=false ` for Child.m1() and call super.m1() inside it.

Example
=======

``` java
public class FooViewModel{
    @AfterCompose
    public void doFoo(){
        //do while AfterCompose
    }
}

public class BarViewModel extends FooViewModel{
    @AfterCompose(superclass=true)
    public void afterComposeBar(){
        //AfterCompose method of super class FooViewModel will be called first.
    }
}

@AfterCompose(superclass=true)
public class ChildViewModel extends BarViewModel{

}
```
