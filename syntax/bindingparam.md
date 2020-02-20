# @BindingParam

Syntax
======

``` java
@BindingParam

@BindingParam("keyString")
```

Description
===========

**Target:** Command method's parameter

**Purpose:** Tell binder to retrieve this parameter with specified key from binding argument on the ZUL.

The annotation is applied to command method's parameter. It declares that the applied parameter should come from binding argument written on the ZUL with the specified key.

> Since 8.0.0

If using ***Client Binding*** to trigger a command, this annotation would convert JSON data into an appropriate object  automatically.

> Since 9.0.1

The value can be omitted if name is the same as the annotated parameter.
```java
@BindingParam String keyString
```

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

> Since 8.0.0

#### Using client binding to trigger a command

```javascript
zkbind.$(someone).command('dataChange', {data:{title: "myData"}});
```

* The above code will send `JSON data` to the command function "dataChange"; this can be automatically converted into an appropriate object using the `BindingParam`.

```Java
public static class DataObject {
    private String title;
    public void setTitle(String title) {
        this.title = title;
    }
    public String getTitle() {return title;}
}

@Command
public void dataChange(@BindingParam("data") DataObject data) {
    // do something here.
}
```
For more information, please visit [ZK Configuration Reference](http://books.zkoss.org/wiki/ZK_Configuration_Reference/zk.xml/The_Library_Properties/org.zkoss.bind.jsonBindingParamConverter.class).
