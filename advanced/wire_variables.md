# Wire Variables

Wire Variables
==============
We can use ` @WireVariable ` to wire variables **from implicit objects or registered variable resolvers** in a ViewModel as we do in a composer (please refer to [ZK Developer's Reference/MVC/Controller/Wire Variables](http://books.zkoss.org/wiki/ZK_Developer%27s_Reference/MVC/Controller/Wire_Variables).
Because ** [BindComposer](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/BindComposer.html) wires those variables for us before calling initial method **. But it will **not**
wire components and listeners automatically like a composer. To achieve it, please refer to [Advance/Wire Components](./wire_components.html) and [Advance/Wire Event Listeners](./wire_event_listeners.html).

Wire from Implicit Objects
--------------------------
Wiring from implicit object is equivalent to calling [Components.getImplicit(org.zkoss.zk.ui.Page, java.lang.String)](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/Components.html#getImplicit%28org.zkoss.zk.ui.Page, java.lang.String%29), by the name specified on `@WireVariable`. If the name is absent and the field or method parameter is of type [Execution](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/Execution.html), [Page](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/Page.html), [Desktop](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/Desktop.html), [Session](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/Session.html), or [WebApp](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/WebApp.html), it still will be wired to the correct implicit object. However, in other cases, an exception will be thrown.

```java
public class FooViewModel {

    @WireVariable
    private Page _page;

    @WireVariable
    private Desktop _desktop;

    @WireVariable
    private Session _sess;

    @WireVariable
    private WebApp _wapp;

    @WireVariable("desktopScope")
    private Map<String, Object> _desktopScope;
}
```

Wire from Variable Resolver
---------------------------
First, you should register variable resolvers. There are two approaches to register a variable resolver: the [VariableResolver](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/select/annotation/VariableResolver.html) annotation or [the variable-resolver directive](http://books.zkoss.org/wiki/ZUML_Reference/ZUML/Processing_Instructions/variable-resolver). Here is the example of registering variable resolvers with annotations.
```java
@VariableResolver({foo1.MyResolver.class, foo2.AnotherResolver.class})
public class FooViewModel {
    // class body
}
```
Then annotate fields or methods with the [WireVariable](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/select/annotation/WireVariable.html) annotation. For example,
```java
@VariableResolver({foo1.MyResolver.class, foo2.AnotherResolver.class})
public class FooViewModel {
    @WireVariable
    Department department;

    @WireVariable
    public void setManagers(Collection<Manager> managers) {
        // method body
    }
}
```

Wire Spring-managed Beans
-------------------------
If you'd like to wire the Spring-managed beans, you usually register the Spring variable resolver, [DelegatingVariableResolver](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zkplus/spring/DelegatingVariableResolver.html). Then, you could annotate `@WireVariable` for wiring a Spring managed bean. For example,
```java
@VariableResolver(org.zkoss.zkplus.spring.DelegatingVariableResolver.class)
public class PasswordViewModel {
    @WireVariable
    private User user;
}
```
[DelegatingVariableResolver](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zkplus/spring/DelegatingVariableResolver.html) is a variable resolver used to retrieve the Spring-managed bean, so the variable will be retrieved and instantiated by Spring.

Notice that the variables are wired before instantiating the component and its children, so you can use them in EL expressions. For example, assume we have a ViewModel as follows.

```java
@VariableResolver(org.zkoss.zkplus.spring.DelegatingVariableResolver.class)
public class UserViewModel {
    @WireVariable
    private List<User> users;

    public ListModel<User> getUsers() {
        return new ListModelList<User>(users);
    }
}
```
Then, you could bind it in the ZUL document. For example,
```xml
<grid model="@load(vm.users)">
```
You might make a ViewModel as a Spring bean then other beans used by the ViewModel can be injected by Spring, but it is **not recommended**. Please refer to [ZK Developer's Reference/MVC/Controller/Wire Variables\#Warning:\_Not\_a\_good\_idea\_to\_have\_Spring\_managing\_the\_composer](http://books.zkoss.org/wiki/ZK_Developer%27s_Reference/MVC/Controller/Wire_Variables#Warning:_Not_a_good_idea_to_have_Spring_managing_the_composer) for the reason.

Wire CDI-managed Beans
======================
The approach to work with CDI is similar to the approach for Spring, except you should register another variable resolver for CDI: [DelegatingVariableResolver](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zkplus/cdi/DelegatingVariableResolver.html).

Wiring Sequence
===============
When wiring variables, the predefined sequence to look for variable resolvers is as follows:

1.  The variable resolver defined in the ZUML document.
2.  The variable resolver annotated registered in the class with the [VariableResolver](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/select/annotation/VariableResolver.html) annotation.
3.  If none of above is found, it looks for [the implicit objects](http://books.zkoss.org/wiki/ZUML_Reference/EL_Expressions/Implicit_Objects), such as session and page.


