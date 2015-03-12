# Pass Arguments to Include Component
When you load a ZUL page using ` Executions.createComponents("mypage.zul", args) ` or ` <include> ` and pass arguments. ZK bind annotation EL expression can not reference those arguments directly because of life cycle issue. What a binder does is a post-processing action after component creation. At the moment of post-processing, it cannot obtain arguments' value. The simplest solution is to add an custom attribute to hold arguments for later reference or use `@ExecutionArgParam` to retrieve in a ViewModel's initial method. Let's see an example.

#### outer.zul
``` xml
<include type="outerPageLiteralValue" src="inner.zul" />
```

-   Here we pass an argument named "type" to an included ZUL.

#### ViewModel for included zul
``` java
public class InnerVM {

    private String typeFromOuter;

    @Init
    public void init(@ExecutionArgParam("type") String type){
        typeFromOuter = type;
    }
    ...
}
```

-   Line 6: Retrieve the passed argument with key "type".

#### inner.zul
``` xml
<div apply="org.zkoss.bind.BindComposer"
        viewModel="@id('vm') @init('org.zkoss.reference.developer.mvvm.advance.InnerVM')">
    <groupbox width="400px">
        Argument passed from outer page:
        <label value="@load(vm.typeFromOuter)"/>
    </groupbox>
</div>
```

Arguments from ViewModel
------------------------

If an argument comes from a ViewModel's property, "src" attribute must also load with data binding and be specified at last attribute for life cycle issue. The reason is similar to previous case, if you specify "src" attribute with a static value, the included page's components are created first but at that moment the argument's value is not determined. Unless both arguments and "src" attribute bound with ViewModel with data binding, they can be processed in the same phase. Then creating included page can access arguments correctly. In addition, ZK only resolves arguments's EL once at component creation, so ViewModel's property change in the future won't reflect on included components.

``` xml
<include type="@load(vm.myArgument)" src="@load('inner.zul')"/>
```

-   The "src" attribute must be specified at last attribute.
-   ZK only resolves arguments's EL once at component creation, so ViewModel's property change in the future won't reflect on included components.

