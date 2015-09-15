# &lt;when>
Syntax
======
```xml
<when test="@load(vm.abc)">
 ...
</when>
```

Description
===========
**Purpose:**

A `<when>` tag is used for `<choose>` like a Java `<switch>` statement in how it lets you decide on a number of alternatives.

Example
=======
```xml
<choose>
	<when test="@load(each lt 3)">
		<div style="color:blue">${each} Template</div>
	</when>
	<when test="@load(each lt 6)">
		<div style="color:yellow">${each} Template</div>
	</when>
	<otherwise>
		<div style="color:red">${each} Template</div>
	</otherwise>
</choose>
```
