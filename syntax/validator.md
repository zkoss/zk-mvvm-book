# @validator

Syntax
======

` @validator(`*`[EL-expression], [arbitraryKey]=[EL-expression]`* `) `

Description
===========

**Target Attribute:** any (except viewModel, validationMessages, form, and event attributes)

**Purpose:** It should be used with ` @bind `, ` @load `, ` @save `. It applies a validator to validate data when saving to ViewModel.

The evaluation result of EL expression should be a [Validator](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/Validator.html) object. You can append arbitrary arguments in key-value pair with comma separated to pass it to the Validator object. Built-in Validator is referenced by a string literal as its name.

***[arbitraryKey]=[EL-expression]***

It's key-value pairs basically. You can write multiple key-value pairs with different key names.

An EL expression without key is set to a default key named **"value"** implicitly.

Due to each annotation has different functions, some annotations may ignore key-value pair expression other than default key, e.g.@id.

***[arbitraryKey]***

It could be any name, it's used as a key for parameter related Java annotation in a ViewModel.

Example
=======

#### Use built-in validator named beanValidator
``` xml
<window id="win" apply="org.zkoss.bind.BindComposer" viewModel="@id('vm') @init(foo.MyViewModel)">
    <textbox value="@bind(vm.user.lastName) @validator('beanValidator')" />
</window>
```

#### Use custom validator
``` xml
<datebox id="cdbox" value="@bind(fx.creationDate) @validator(vm.creationDateValidator)"/>
```
