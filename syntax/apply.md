# &lt;apply>
Syntax
======

```xml
<apply templateURI="..." />
```

```xml
<apply template="template1" arg="value">
	<template name="template1">
	    ...
	    <label value="@load(arg)" />
	</template>
</apply>
```

Description
===========

**Purpose:**

 The apply tag allows you to choose which `<template>` to be
 applied. You specify the template's name using the **template** attribute or the template's URI using the **templateURI** attribute.

 There are two ways to pass parameters to the applied shadow:
 First, you can use an additional attribute in ZUL, like:
 `<apply templateURI="/WEB-INF/mypage" arg="something" />`

 Second, you can use the query string:
 `<apply templateURI="/WEB-INF/mypage?arg=something" />`

 With the query string, you can pass only the String values.
 The parameter can be accessed with the <tt>arg</tt> variable in EL expressions.

Example
=======

```xml
<apply templateURI="@load(vm.templateURI)"/>
```

```xml
<apply template="@load(vm.template)" arg="@ref(vm.xxx)">
	<template>Default</template>
	<template name="a">
	    <label value="@load(arg)" />
	</template>
	<template name="b">...</template>
</apply>
```
