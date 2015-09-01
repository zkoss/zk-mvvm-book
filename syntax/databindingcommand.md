# @command

Syntax
======

` @command(`*`[EL-expression], [arbitraryKey]=[EL-expression]`*`) `

Description
===========

**Target Attribute:** event attributes (e.g. onClick, onOK)

**Purpose:** Specify which command to execute when the event fires

You can pass arbitrary arguments in key-value pairs with comma separated.

Notice that **value** is a reserved word, you should avoid to use it as an arbitraryKey.

***[arbitraryKey]=[EL-expression]***

It's key-value pairs basically. You can write multiple key-value pairs with different key names.

An EL expression without key is set to a default key named **"value"** implicitly.

Due to each annotation has different functions, some annotations may ignore key-value pair expression other than default key, e.g.@id.

***[arbitraryKey]***

It could be any name, it's used as a key for parameter related Java annotation in a ViewModel.

Example
=======

``` xml
<button label="Save" onClick="@command('saveOrder')"/>

<button label="Delete" onClick="@command(empty vm.selected.id ? 'deleteOrder' : 'confirmDelete')"/>

<button label="Index" onClick="@command('showIndex', index=10, keyword='myKeyword')"/>
```
