# Reference Binding

> Available for ZK: PE, EE

> Since 6.0.1

Make an expression reference
============================
Reference binding allows us to reference an EL expression with a customized name. We can use this reference in another EL expression nested in the component that is bound to this reference. Typically, we use reference binding to shorten expressions or to modularize the view<sub>[1]</sub>.

Steps to use this feature:
1.  Bind a custom attribute on a component with ` @ref `.
2.  Use the custom attribute in other EL expressions that are nested in children components

[1]: When using this feature, you have to avoid binding to a single variable, read [limitation](./property_binding.html#limitation) for more detail.

#### Basic usage example
```xml
<window apply="org.zkoss.bind.BindComposer" viewModel="@id('vm') @init('foo.MyVM')">
    <vlayout p="@ref(vm.person)">
        <hlayout>
            First Name: <textbox value="@bind(p.firstName)" />
        </hlayout>
        <hlayout>
            Last Name: <textbox value="@bind(p.lastName)" />
        </hlayout>
    </vlayout>
</window>
```
As shown in the example above, we add a reference `@ref` to **vm.person** with a name **p** to `vlayout`. This way, **p** inside the `vlayout` can be used by other EL expressions.

#### Modularize view example
```xml
<window apply="org.zkoss.bind.BindComposer" viewModel="@id('vm') @init('foo.MyVM')">
    <include p="@ref(vm.person1)" src="person.zul"/>
    <include p="@ref(vm.person2)" src="person.zul"/>
</window>
```

#### Modularize view example - person.zul
```xml
<vlayout>
    <hlayout>
        First Name: <textbox value="@bind(p.firstName)"/>
    </hlayout>
    <hlayout>
        Last Name: <textbox value="@bind(p.lastName)"/>
    </hlayout>
</vlayout>
```
In the example above, we have a view - person.zul which binds components to **p.firstName** and **p.lastName**. This view could be reused by other views that provides a **p** reference.
