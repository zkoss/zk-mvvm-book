# @save

Syntax
======

` @save(`*`[EL-expression], [conditionKeyword]=[EvaluateOnce EL-expression]`*`) `

Description
===========

**Target Attribute:** any save-allowed attributes (except viewModel, validationMessages)

**Purpose:** Restrict binder to save data to ViewModel only, no loading.

You usually use this syntax when you want to save and load data in different conditions, you should write both ` @save ` and ` @load ` in an attribute. You have to use it in form binding to save upon a command.

***[conditionKeyword]=[EvaluateOnce EL-expression]***

This expression is optional unless you want to save or load upon a command.

***[conditionKeyword]***

It could be one of [ **before | after** ]

***[EvaluateOnce EL-expression]***

The evaluation result must be one or more command name.

Command name must correspond to the name specified in Java annotation ` @Command ` in a ViewModel.

Example
=======

#### Basic usage
``` xml
<textbox value="@load(vm.person.name) @save(vm.person.name, before='save')"/>

<textbox value="@load(vm.person.name) @save(vm.person.name, before={'save', 'update'})"/>
```

#### Saving and loading form attribute
``` xml
<textbox value="@save(vm.number) @load(vm.number, after='cmd')" />
```
