# @ref
> Available for ZK: PE, EE

> Since6.0.1

Syntax
======

` @ref(`*`[EL-expression]`*`) `

Description
===========

**Target Attribute:** custom attribute.

**Purpose:** Create a reference to an EL expression that can be used in another EL expression.

Create this binding on a component's custom attribute whose name can be referred and used in another EL expression. The EL expression that refers to the reference must be the children of the component that contains the reference binding. This feature is useful in shortening EL expressions by referring it in another expression.

Example
=======

``` xml
<div p="@ref(vm.selectedPerson)">
    <label value="@load(p.id)" />
    <label value="@load(p.name)" />
</div>
```
