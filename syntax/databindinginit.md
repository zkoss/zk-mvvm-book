# @init

Syntax
======

` @init(`*`[EvaluateOnce EL-expression], [arbitraryKey]=[EL-expression]`*`) `

Description
===========

**Target Attribute:** any

**Purpose:** Initialize an attribute's value and no reload during user interaction.

The EL expression in this annotation is only evaluated once when binder parses it. When you use it on "viewModel" attribute, binder will try to resolve the EL expression as a class and create an instance of it.

In "form" attribute, the result of evaluating the EL expression should be a [Form](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/Form.html) object.

For other attributes, binder initializes their value with EL evaluation result.

When using on "viewModel" attribute, we can pass arguments in key-value pairs with comma separated. And we can get the arguments in a initial method of ViewModel by [`@BindingParam`](/syntax/bindingparam.html).

**_[arbitraryKey]=[EL-expression]_**

It's key-value pairs basically. You can write multiple key-value pairs with different key names.

An EL expression without key is set to a default key named **"value"** implicitly.

Due to each annotation has different functions, some annotations may ignore key-value pair expression other than default key, e.g.@id.

***[arbitraryKey]***

It could be any name, it's used as a key for parameter related Java annotation in a ViewModel.

Example
=======

#### Usage example in viewModel attribute
``` xml
<window apply="org.zkoss.bind.BindComposer" viewModel="@id('vm') @init('foo.ChildrenMenuVM')" >
</window>
```

#### Usage example in form attribute
``` xml

<vbox form="@id('fx') @init(vm.myForm) @load(vm.person) @save(vm.person, before='save')">
</vbox>
```

#### Pass arguments
``` xml
    <window  width="400px" apply="org.zkoss.bind.BindComposer"
    viewModel="@id('vm') @init('org.zkoss.reference.developer.mvvm.databinding.InitVM', arg1='myValue')">
        ...
    </window>
```

``` java
public class InitVM {

    @Init
    public void init(@BindingParam("arg1") String arg1){
        System.out.println("arg1: "+arg1);
    }
}
```
