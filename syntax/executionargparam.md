# @ExecutionArgParam

Syntax
======

``` java
@ExecutionArgParam

@ExecutionArgParam("keyString")
```

Description
===========

**Target:** A method's parameter (for initial and command methods)

**Purpose:** Tell binder to retrieve this parameter with specified key from the current execution's argument.

The annotation is applied to initial (or command) method's parameter. It declares that the applied parameter should come from the current execution's argument with specified key.

> Since 9.0.1

The value can be omitted if name is the same as the annotated parameter.
```java
@ExecutionArgParam String keyString
```

Example
=======

Assume we want to pass an argument to an included ZUL that uses a ViewModel: ` ExecutionParamVM`.

#### outer ZUL
``` xml
<window >
    <include arg1="foo" src="executionparam-inner.zul"/>
</window>
```

We use annotation to retrieve execution's argument with key "arg1".

``` java
public class ExecutionParamVM {

    private String arg1;

    @Init
    public void init(@ExecutionArgParam("arg1") String arg1) {
        this.arg1 = arg1;
    }
    //setter, getter, and others
}
```

#### executionparam-inner.zul
``` xml
<vbox apply="org.zkoss.bind.BindComposer" viewModel="@id('vm') @init('foo.ExecutionParamVM')">
    <label value="@load(vm.arg1)"/>
</vbox>
```

-   The label will display "foo".

