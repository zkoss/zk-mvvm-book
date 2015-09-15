# &lt;otherwise>
Syntax
======
```xml
<otherwise>
 ...
</otherwise>
```

Description
===========
**Purpose:**

A switch statement has a default clause to specify a default action, similar to how `<choose>` has `<otherwise>` as default clause.

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
