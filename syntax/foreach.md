# &lt;forEach>
Syntax
======
```xml
<forEach begin="0" end="3" step="2" var="x">
  ...
</forEach>
```

```xml
<forEach items="@load(vm.model)" >
  <label value="@load(each)"/>
</forEach>
```

Description
===========
**Purpose:**

The basic iteration tag, accepting many different collection types and supporting sub-setting and other functionality like JSTL's `forEach` .

Specifying the collection by using the **items** attribute, and the current item is available through a variable named by the **var** attribute.


Example
=======
```xml
<forEach begin="@load(vm.begin)" end="@load(vm.end)" step="@load(vm.step)" var="x">
	<label value="${x}"/>
</forEach>
```

```xml
<forEach items="@load(vm.model)">
	<label value="@load(each)"/>
</forEach>
```
