# @command

Syntax
======

` @command(`*`[EL-expression], [arbitraryKey]=[EL-expression]`*`) `

***Simplified syntax***

> Since 9.5.0 

` @(`*`[EL-expression], [EL-expression]`*`) `

Description
===========

**Target Attribute:** event attributes (e.g. onClick, onOK)

**Purpose:** Specify which command to execute when the event fires

You can pass arbitrary arguments in key-value pairs with a comma to separate.

Notice that **value** is a reserved word, you should avoid using it as an arbitraryKey.

***[arbitraryKey]=[EL-expression]***

Basically, it uses key-value pairs. You can write multiple key-value pairs with different key names.

An EL expression without key is set to a default key named **"value"** implicitly.

Since each annotation has different functions, some annotations may ignore key-value pair expressions other than default key, e.g.@id.

***[arbitraryKey]***

It could be any name; it's used as a key for parameter related Java annotation in a ViewModel.

***Simplified syntax***

We can use simplified syntax to do command binding, which means we don't need specify any 'arbitraryKey'.

The command method in ViewModel will be triggered with positional arguments.

Notice that it only works in the matched attribute format: "on" + action (ex. onClick, onChange, ...).

Example
=======

``` xml
<button label="Save" onClick="@command('saveOrder')"/>

<button label="Delete" onClick="@command(empty vm.selected.id ? 'deleteOrder' : 'confirmDelete')"/>

<button label="Index" onClick="@command('showIndex', index=10, keyword='myKeyword')"/>

<!-- Simplified syntax --> 
<button label="New" onClick="@('newOrder', 10, 'newKeyword')" />
```
