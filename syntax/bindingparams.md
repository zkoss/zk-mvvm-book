# @BindingParams
> Since 9.1.0

> Don't get confused with @BindingParam.

Syntax
======

``` java
@BindingParams
```

Description
===========

**Target:** Command method's parameter

**Purpose:** Tell binder to retrieve this parameter with specified object from binding arguments on the ZUL.

The annotation is applied to command method's parameter. It declares that the applied parameter should come from binding arguments written on the ZUL with the specified keys.

The object should have a no-arg constructor, or the object can't be created. Passing all properties is not necessary. The properties not passed will be the default value.

Though both annotations can be applied to an object, it is slightly different between `@BindingParams` and `@BindingParam`. Using `@BindingParams` needs to pass some properties and will be combined into one object, while using `@BindingParam` should pass a JSON object from client binding.

Example
=======

#### A plain old Java object (POJO)
``` java
public class Message {
    private String myKey;
    private String content;

    public String getMyKey() {
        return myKey;
    }

    public void setMyKey(String myKey) {
        this.myKey = myKey;
    }

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }
}
```

#### Command binding that pass parameters
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
public void popupMessage(@BindingParams Message message) {
    // method body
    // message.getMyKey() or message.getContent()
}
```

- The message will be a `Message` object containing properties from binding parameters
