# &lt;if>
Syntax
======
```xml
<if test="@load(vm.visible)">
 ...
</if>
```

Description
===========
**Purpose:**

The conditional execution of its body according to the value of the **test** attribute


Example
=======
```xml
<if test="@load(vm.rootVisible)">
	<if test="@load(vm.visible)">
		<label use="MyLabel1"/>
	</if>
	<label use="MyLabel2"/>
</if>
```
