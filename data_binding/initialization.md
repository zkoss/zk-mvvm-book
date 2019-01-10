# Initialization
We can initialize any component's attribute by initial binding: ` @init `. It loads ViewModel's property once at the beginning then the property won't be synchronized by binder during user interaction. But EL evaluation's result we expect in initial binding annotation is different for attribute “viewModel” and “form”.

@init at viewModel Attribute
============================
The first place we usually use this annotation is to assign a ViewModel to a component in viewModel attribute. When using in viewModel attribute, we should specify the **full-qualified class name** in a string literal in ` @init `.
```xml
<window apply="org.zkoss.bind.BindComposer" viewModel="@id('vm') @init('foo.MyViewModel')">
</window>
```
The composer will resolve the string 'foo.MyViewModel' and create a object of it.

@init at Component's Attribute
==============================
It's common to use it to initialize a component's attribute with a constant value or a ViewModel's property. The binder loads it once and doesn't synchronize it afterward during following user interaction.
``` xml
    <label value="@init(vm.message)"/>

    <label value="@init(123)"/>

    <checkbox checked="@init(true)"/>
```

@init at Form Binding
=====================
We can also use the initial binding on the form binding to fill in predefined value.
```xml
<div form="@id('fx') @init(vm.defaultOrder) @load(vm.order) @save(vm.order, before='compute')">

</div>
```
We'll describe more advanced usage with ` @init ` in section [Data Binding/Form Binding](./form_binding.html).
