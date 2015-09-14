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

The `<choose>` tag works like a Java switch statement in that it lets you choose between a number of alternatives. Where the switch statement has case statements, the `<choose>` tag has `<when>` tags. A a switch statement has default clause to specify a default action and similar way `<choose>`
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
