# @bind

Syntax
======

` @bind(`*`[EL-expression]`*`) `

Description
===========

**Target Attribute:** all (except viewModel, validationMessages, form)

**Purpose:** Specify that binder can both save and load data

It's like a shortcut annotation that combines both ` @save ` and ` @load `. When your saving and loading don't depend on different condition, it's suggested to use this annotation.

Example
=======

``` xml
<textbox id="t2" value="@bind(vm.user.account)" />

<doublebox id="pbox" value="@bind(vm.selected.price)"/>

<datebox id="sdbox" value="@bind(vm.selected.shippingDate)"/>
```
