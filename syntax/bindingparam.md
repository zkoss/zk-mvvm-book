# @BindingParam

Syntax
======

``` java
@BindingParam("keyString")
```

Description
===========

**Target:** Command method's parameter

**Purpose:** Tell binder to retrieve this parameter with specified key from binding argument on the ZUL.

The annotation is applied to command method's parameter. It declares the applied parameter should come from binding argument written on the ZUL with specified key.

Example
=======

#### Command binding that pass parameters*
``` xml
<listbox model="@load(vm.items)" selectedItem="@bind(vm.selected)" hflex="true" height="300px">
    <listhead>
        <listheader label="Name"/>
        <listheader label="Price" align="center"/>
        <listheader label="Quantity" align="center"/>
    </listhead>
    <template name="model" var="item">
        <listitem onMouseOver="@command('popupMessage', myKey='myValue', content=item.description)">
            <listcell label="@bind(item.name)"/>
            <listcell label="@bind(item.price)"/>
            <listcell label="@bind(item.quantity)"/>
        </listitem>
    </template>
</listbox>
```

#### Command method in ViewModel with binding parameter
``` java
@Command
public void popupMessage(@BindingParam("myKey") String target, @BindingParam("content") String content) {
    // method body
}
```

-   The `target `'s value is “myValue”, and ` content`'s is object item's description property.
