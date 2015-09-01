# @global-command

Syntax
======

` @global-command(`*`[EL-expression], [arbitraryKey]=[EL-expression]`*`) `

Description
===========

**Target Attribute:** event attributes (e.g. onClick, onOK)

**Purpose:** Specify which global command to execute when the event fires

If you use this binding with a local command binding, remember that local command is always executed first.

You can pass arbitrary arguments in key-value pairs with comma separated.

***[arbitraryKey]=[EL-expression]***

It's key-value pairs basically. You can write multiple key-value pairs with different key names.

An EL expression without key is set to a default key named **"value"** implicitly.

Due to each annotation has different functions, some annotations may ignore key-value pair expression other than default key, e.g.@id.

***[arbitraryKey]***

It could be any name, it's used as a key for parameter related Java annotation in a ViewModel.

Example
=======

``` xml
<button label="Save" onClick="@command('saveOrder') @global-command('refresh')"/>

<button label="ShowAll" onClick="@global-command('show')"/>

<button label="Index" onClick="@command('showIndex') @global-command('showIndex', index=10, keyword='myKeyword')"/>
```
