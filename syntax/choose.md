# &lt;choose>
Syntax
======
```xml
<choose>
 ...
</choose>
```

Description
===========
**Purpose:**

The `<choose>` tag works like a Java switch statement, as it lets you choose between a number of alternatives. Where the switch statement has case statements, the `<choose>` tag has `<when>` tags. A switch statement has a default clause to specify a default action, similar to how `<choose>`
has `<otherwise>` as default clause.


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
