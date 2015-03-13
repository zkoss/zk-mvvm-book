# @SelectorParam

Syntax
======

``` java
@SelectorParam("#componentId")

@SelectorParam("tagName")

@SelectorParam(".className")

@SelectorParam(":root")

@SelectorParam("button[label='Submit']")

@SelectorParam("window > button")
```

For selector syntax, please refer to [SelectorComposer](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/select/SelectorComposer.html)

Description
===========

**Target:** A method's parameter (for initial and command methods)

**Purpose:** To identify that a method's parameter should be retrieved from view component of the binder.

The **value** element is the selector to find components. It uses [Selectors](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/select/Selectors.html) to select the components. The base component of the selector is the view component of the binder, the component which uses ViewModel.

If the parameter type is a Collection, binder passes the result directly. Otherwise it passes the first result or null if no result.

Example
=======

``` xml
    <vbox apply="org.zkoss.bind.BindComposer" viewModel="@id('vm') @init('foo.SelectorParamVM')">

        <hbox><label id="message" /></hbox>
        <hbox><label /></hbox>
        <hbox><label /></hbox>
        <hbox><label /></hbox>

        <button id="cmd" label="cmd" onClick="@command('cmd')" />
    </vbox>
```

#### Example to pass components by selector
``` java
public class SelectorParamVM {

    @Command
    public void cmd(@SelectorParam("label") LinkedList<Label> labels, @SelectorParam("#message") Label msg) {
        for (int i = 0; i < labels.size(); i++) {
            labels.get(i).setValue("Command " + i);
        }
        msg.setValue("msg in command");
    }
}
```
