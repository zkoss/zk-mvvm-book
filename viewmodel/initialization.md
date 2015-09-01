# Initialization

## Initial Method
The binder is responsible for creating and initializing a ViewModel instance. If you want to perform your own initialization in a ViewModel, you can declare your own **initial method** by annotating a method with `@Init`. The Binder will invoke this method when initializing a ViewModel. Each ViewModel can only have one initial method.
```java
public class MyViewModel {

    @Init
    public void init() {
        //initialization code
    }
}
```
This annotation has an attribute named "superclass", if you set it to "true". The binder will look for the initial method of ViewModel's parent class and invoke it first if it exists.
```java
public class ParentViewModel {

    @Init
    public void parentInit() {
        //initialization code
    }
}

public class ChildViewModel extends ParentViewModel {

    /* ParentViewModel's initial method is invoked first then ChildViewModel's. */
    @Init(superclass=true)
    public void childInit() {
        //initialization code
    }
}
```
Please notice that child class's initial method **should not override parent class's initial method**, or parent's initial method won't be called and child's will be called twice.

##Apply on Class
> Since 6.0.1

If super has an init methold but its ChildViewModel doesn't, you can add `@Init(superclass=true)` on the ChildViewModel to use super's init.
```java
public class ParentViewModel {
    @Init
    public void init(){}
}

@Init(superclass=true)
public class ChildViewModel extends ParentViewModel {
}
```
The initial method can retrieve various context object by applying annotation on its parameter, please refer [Advanced/Parameters](../advanced/parameters.html).
