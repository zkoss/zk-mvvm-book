# UI Template Injection

In ZK 8, `<template>` is our recommended form to reuse a view pattern composed by a group of components. Putting components into `<template>` can make them reusable easily by `<apply>`. It usually involves 2 steps:

- Define a template
- Apply a template

## Define a template
In ZK 8, you can put a `<template>` inside any component. Defining a template will not create any component until you apply it. You can define a template like:

```xml
<div>
    <template name="layout">
    <!-- UI components or shadow components -->
    </template>
</div>
```
or a path of a zul

```xml
<div>
    <template name="layout" src="/mytemplate.zul"/>
</div>
```
The usage is the same as we have mentioned in previous chapters. But we can also use the following tags to describe a component creation logic based on certain conditions.

## Apply a Template

When we apply a template, ZK will create the components inside the template upon its logic and insert those components into the position of `<apply>` tag. Therefore, we also call it Template Injection.

We usually apply a template with its name like:

`<apply template="layout"/>`
Or apply with a path of a zul:

`<apply templateURI="/chapter1/banner.zul"/>`



Turn into Templates

To reuse the `<forEach>`, we turn it into a template named iterate first.

```xml
<navbar id="navbar" orient="horizontal" collapsed="false" onSelect="@command('navigate')" >
    <apply template="iterate" menuItems="@ref(vm.menuHierarchy)"/>
</navbar>
<template name="iterate">
    <forEach items="@load(vm.menuHierarchy)">
        <choose>
            <when test="@load(empty each.subMenus)">
                <navitem label="@load(each.label)" />
            </when>
            <otherwise>
                <nav label="@load(each.label)" iconSclass="@load(each.iconSclass)"/>
            </otherwise>
        </choose>
    </forEach>
</template>
```
Line 2: We pass a parameter by menuItems="@ref(vm.menuHierarchy)". Therefore, we can access the menu list in `<forEach>` by items="@load(menuItems)".
In this simple case (just 2 choices), we can re-write it in a simpler way by creating 2 templates for nav and navitem respectively.

```xml
<template name="menu">
    <nav label="@load(menuItem.label)" iconSclass="@load(menuItem.iconSclass)"/>
</template>
<template name="menuitem" >
    <navitem label="@load(menuItem.label)" />
</template>
```
Then replace `<choose>`/ `<when>`/ `<otherwise>` with ternary operator ? like:

```xml
<template name="iterate">
    <forEach items="@load(menuItems)">
        <apply template="@load(empty each.subMenus?'menuitem':'menu')" menuItem="@ref(each)"/>
    </forEach>
</template>
```

Apply a Template inside a Template

Everything is fine so far except that those sub-menu are not rendered. That's because in template menu, we just render a menu node itself as a `<nav>` and don't render its sub-menu. A node in a sub-menu is also a menu node, and it could also have a sub-menu. We still need to render a sub-menu node like we would do for a menu node, using a control structure. The best thing is: we don't need to repeat ourselves in template menu again. We can just apply the template iterate to iterate a collection of menu nodes recursively.

All 3 templates used in this example

```xml
<template name="menu">
    <nav label="@load(menuItem.label)" iconSclass="@load(menuItem.iconSclass)">
        <apply template="iterate" menuItems="@ref(menuItem.subMenus)"/>
    </nav>
</template>
<template name="iterate">
    <forEach items="@load(menuItems)">
        <apply template="@load(empty each.subMenus?'menuitem':'menu')" menuItem="@ref(each)"/>
    </forEach>
</template>
<template name="menuitem" >
    <navitem label="@load(menuItem.label)" />
</template>
```
Line 3: Apply the previous template iterate here to traverse each menu node and render them.


# Using Shadow Elements inside a Template Takes Heavy Rendering Cost

The usage like below:

```xml
 <listbox model="@load(vm.listModel)">
    <template name="model">
        <apply template="listitem1" />
    </template>
</listbox>

<template name="listitem1">
    <listitem focus="@load(self.selected)" >
        <apply template="listcell11" />
        <apply template="listcell12" />
    </listitem>
</template>
```

The long processing time is caused by shadow elements positioning themselves relative to the other shadow elements located in the same shadow root element.
Since every shadow element inside the same parent needs to sync its position in the component tree based on its siblings. Adding or removing shadow elements causes "all other shadow elements at the same level" to also recalculate, causing the cumulative processing times.

If you want to determine components used in a template in runtime upon a value, please use [@template](../syntax/template.html). See [Combine with Dynamic Template](../data_binding/children_binding.html).