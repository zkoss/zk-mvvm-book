# @id

Syntax
======

` @id(`*`[EvaluateOnce EL-expression]`*`)`

Description
===========

**Target Attribute:** viewModel, form, validationMessages

**Purpose:** To give an id to current binding target which can be used to reference its properties in the binding annotation of child components.

We suggest you use a string literal in EL expression, as binder only evaluate this annotation's EL expression once to determine ViewModel's id, and this EL expresion is also used in other child component's ZK bind annotation. If it's not a fixed value, it will cause incorrect evaluation results.

When we use it in "validationMessages" attribute, it gives an id to reference validation message holder.

When we use it in "form" attribute, it gives an id to reference form's middle object. Form status variable's naming is: `[middleObjectId]Status`

Example
=======

#### Usage in viewModel attribute
``` xml
<window apply="org.zkoss.bind.BindComposer" viewModel="@id('vm') @init('foo.ChildrenMenuVM')"
	validationMessages = "@id('vmsgs')">
</window>
```

**Usage in form attribute**

``` xml
<grid form="@id('fx') @load(vm.user) @save(vm.user,before='register')">
</grid>
```
