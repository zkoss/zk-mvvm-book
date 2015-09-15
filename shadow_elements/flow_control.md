# Flow Control
To use flow control in shadow elements, we can use the following shadow element tags:
- `<if>`: allows the conditional execution of its body according to the value of the test attribute.
- `<choose>/<when>/<otherwise>`: they are used for logic and flow control, such as Java's switch/case/default statement.


# Work with UI components
Continuing with the example from the previous chapter, we will render a menu node upon its sub-menu existence. First we specify whether or not to show *Home* icon. And if a menu node has a sub-menu, we render it by `<nav>`; otherwise render it by `<navitem>`. To achieve this, we have to use shadow components for flow control, `<if>`, `<choose>`, `<when>`, and `<otherwise>`.

- The `<if>` is just like Java **if** statement.
- The `<choose>` is just like Java **switch** statement.
- The `<when>` is like **case** statement, and you should specify a data binding expression on test attribute to be evaluated.
- The `<otherwise>` works like **default** case in a switch statement to specify a default action.
Now, we can test whether a menu node has a subMenus or not by `test="@load(empty each.subMenus)"`. If it does, create a `<nav>`, otherwise create a `<navitem>`.

```xml
<navbar id="navbar" orient="horizontal" collapsed="false">
    <if test="@load(showHome)">
        <navitem label="Home" iconSclass="z-icon-home" />
    </if>
    <forEach items="@load(menuItems)">
        <choose>
            <when test="@load(empty each.subMenus)">
                <navitem label="@load(each.label)" />
            </when>
            <otherwise>
                <nav label="@load(each.label)" iconSclass="@load(each.iconSclass)"/>
            </otherwise>
        </choose>
    </forEach>
</navbar>
```

But this way we only create a `<nav>` without its `<navitem>`. We need to traverse each node in its sub-menu and create correponding components (`<nav>` or `<navitem>`) which is just like what we would do now for each menu-node. It means we need to reuse this `<forEach>` and its child elements.
