# @command

Syntax
======

` @command(`*`[EL-expression], [arbitraryKey]=[EL-expression]`*`) `

Description
===========

**Target Attribute:** event attributes (e.g. onClick, onOK)

**Purpose:** Specify which command to execute when the event fires

You can pass arbitrary arguments in comma separated key-value pairs.

Notice that **value** is a reserved word, you should avoid using it as an arbitraryKey.

***[arbitraryKey]=[EL-expression]***

Basically, it uses key-value pairs. You can write multiple key-value pairs with different key names.

An EL expression without key is set to a default key named **"value"** implicitly.

Since each annotation has different functions, some annotations may ignore key-value pair expressions other than default key, e.g.@id.

***[arbitraryKey]***

It could be any name; it's used as a key for parameter related Java annotation in a ViewModel.

Example
=======

``` xml
<button label="Save" onClick="@command('saveOrder')"/>

<button label="Delete" onClick="@command(empty vm.selected.id ? 'deleteOrder' : 'confirmDelete')"/>

<button label="Index" onClick="@command('showIndex', index=10, keyword='myKeyword')"/>
```
