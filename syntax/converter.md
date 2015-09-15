# @converter

Syntax
======

` @converter(`*`[EL-expression], [arbitraryKey]=[EL-expression]`*`) `

Description
===========

**Target Attribute:** any (except viewModel, validationMessages, form, and event attributes)

**Purpose:** It should be used with ` @bind `, ` @load `, ` @save `. It applies a converter to convert data during transformation between UI components and ViewModel.

The evaluation result of EL expression should be a [Converter](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/Converter.html) object. You can append arbitrary arguments in key-value pair with a comma for separation to pass it to the Converter object. Built-in Converter is referenced by a string literal as its name.

***[arbitraryKey]=[EL-expression]***

Basically, it uses key-value pairs. You can write multiple key-value pairs with different key names.

An EL expression without a key is set to a default key named **"value"** implicitly.

Since each annotation has different functions, some annotations may ignore key-value pair expressions other than the default key, e.g.@id.

***[arbitraryKey]***

It could be any name. It's used as a key for parameter related Java annotation in a ViewModel.

Example
=======

#### Use built-in converter named formatedNumber
``` xml
<label value="@load(item.price) @converter('formatedNumber', format='###,##0.00')"/>
```

#### Use custom converter
``` xml
<label value="@load(vm.selected.totalPrice) @converter(vm.totalPriceConverter)"/>
```
