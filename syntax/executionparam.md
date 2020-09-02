# @ExecutionParam

Syntax
======

``` java
@ExecutionParam

@ExecutionParam("keyString")
```

Description
===========

**Target:** A method's parameter (initial method and command method)

**Purpose:** Tell binder to retrieve this parameter with specified key from the current execution's attribute.

The annotation is applied to initial (or command) method's parameter. It declares that the applied parameter should come from the current execution's attribute with specified key.

> Since 9.5.0

The value can be omitted if name is the same as the annotated parameter.
```java
@ExecutionParam String keyString
```

Example
=======

Assume we want to pass an object by execution's attribute to an included ZUL that uses a ViewModel: ` ExecutionParamVM`.

#### outer ZUL
``` xml
<window id="w2">
    <zscript>
        void doClick() {
            org.zkoss.zk.ui.Execution ex = org.zkoss.zk.ui.Executions.getCurrent();
            ex.setAttribute("param1", "abc");
            inc.src = "executionparam-inner.zul";
        }
    </zscript>
    <button label="do include" onClick="doClick()"/>
    <include id="inc"/>
</window>
```

We use annotation to retrieve execution's attribute with key "param1".

``` java

public class ExecutionParamVM {
    private String param1;

    @Init
    public void init(@ExecutionParam String param1) {
        this.param1 = param1;
    }
    // setter, getter, and others
}
```
#### executionparam-inner.zul
``` xml
    <vbox apply="org.zkoss.bind.BindComposer" viewModel="@id('vm') @init('foo.ExecutionParamVM')">
        <label value="@load(vm.param1)"/>
    </vbox>
```

-   The label will display "abc".

