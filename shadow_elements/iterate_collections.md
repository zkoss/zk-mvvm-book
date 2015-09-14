# Iterate Collections
We can use the `<forEach>` tag to iterate over a collection of objects (notice the captalized 'E').

Specifying the collection by using the **items** attribute, and you can access the current item through a variable named specified at the **var** attribute. Also, we can use the following attributes - **begin**, **end**, and **step** to specify the iteration start point, end point, and iteration step.

#Work with UI components
Starting from the basic, we can just render the list of menu node with `<forEach>`and `<navitem>`. The `<forEach>` can iterate over a collection of objects. Specifying the collection by the items attribute and access the current item through a variable named each. Or you can define the current item variable by the var attribute e.g. if you write `<forEach items="@load(menuItems)" var="menu">` then you should use menu in a data binding expression like `<navitem label="@load(menu.label)">`.

```xml
<navbar id="navbar" orient="horizontal" collapsed="false">
    <forEach items="@load(menuItems)">
        <navitem label="@load(each.label)"
            iconSclass="@load(each.iconSclass)">
        </navitem>
    </forEach>
</navbar>
```

But this way only renders each menu nodes as a `<navitem>`. However, we need to render those menu nodes with a sub-menu as a `<nav>`.
