# @load

Syntax
======

` @load(`*`[EL-expression], [conditionKeyword]=[EvaluateOnce EL-expression]`*`) `

Description
===========

**Target Attribute:** any (except viewModel, validationMessages)

**Purpose:** Restrict binder to load data from ViewModel only, not save back

For some attributes that don't save data back to the ViewModel like listbox's model or label's value, you can also write ` @bind ` or ` @load `.

***[conditionKeyword]=[EvaluateOnce EL-expression]***

This expression is optional unless you want to save or load upon a command.

***[conditionKeyword]***

It could be one of [ '''before | after ''' ]

***[EvaluateOnce EL-expression]***

The evaluation result must be one or more command name.

Command name must correspond to the name specified in Java annotation ` @Command ` in a ViewModel.

Example
=======

``` xml
<label value="@load(vm.user.id)"/>

<label value="@load(vm.user.permission, after='showPermission')"/>

<label value="@load(vm.user.permission, after={'showPermission', 'showAll'})"/>

<label value="@load(vm.user.action, before='process')"/>
```
