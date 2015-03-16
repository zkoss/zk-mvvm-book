# Converter
Converter performs two way conversion between ViewModel's property and UI component attribute value. It converts data to component attribute when loading ViewModel's property to components and converts back when saving to ViewModel. It's quite common requirement to display data in different format in different context. Using converter developers can achieve this without actually changing the real data in a ViewModel.

Implement a Converter
=====================
Developers can create a custom converter by implementing the [Converter](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/Converter.html) interface. The method
` coerceToUi() ` is invoked when loading ViewModel's property to component and its return type should correspond to bound component attribute's value<sub>[1]</sub>. The ` coerceToBean() ` is invoked when saving. If you only need to one way conversion, you can leave unused method empty.

[1]: Since 6.0.1, a converter can return the [IGNORED_VALUE](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/Converter.html#IGNORED_VALUE) to indicate that binder should ignore to load the value to component. It is useful if you want to do some extra method call on a component after a command or a value change notification.

The following is how built-in converter 'formattedDate' implement.
```java
public class MyDateFormatConverter implements Converter {
    /**
     * Convert Date to String.
     * @param val date to be converted
     * @param comp associated component
     * @param ctx bind context for associate Binding and extra parameter (e.g. format)
     * @return the converted String
     */
    public Object coerceToUi(Object val, Component comp, BindContext ctx) {
        //user sets format in annotation of binding or args when calling binder.addPropertyBinding()
        final String format = (String) ctx.getConverterArg("format");
        if(format==null) throw new NullPointerException("format attribute not found");
        final Date date = (Date) val;
        return date == null ? null : new SimpleDateFormat(format).format(date);
    }

    /**
     * Convert String to Date.
     * @param val date in string form
     * @param comp associated component
     * @param ctx bind context for associate Binding and extra parameter (e.g. format)
     * @return the converted Date
     */
    public Object coerceToBean(Object val, Component comp, BindContext ctx) {
        final String format = (String) ctx.getConverterArg("format");
        if(format==null) throw new NullPointerException("format attribute not found");
        final String date = (String) val;
        try {
            return date == null ? null : new SimpleDateFormat(format).parse(date);
        } catch (ParseException e) {
            throw UiException.Aide.wrap(e);
        }
    }
}
```
- We retrieve “format” parameter's value by ` ctx.getConverterArg("format") `. This allows you to pass in arbitrary parameters.

According to above code, we can pass a “format” parameter to 'formattedDate' converter.
```xml
<label value="@load(item.creationDate) @converter(vm.myConverter, format='yyyy/MM/dd')"/>
```

Use Custom Converter
====================
The most common way to apply a converter is to bind a component attribute to **ViewModel's property which is a custom converter** or you can also specify custom * converter's full-qualified class name*.

#### Return a converter as a property
```java
public class MyViewModel{
    private Converter MyConverter = new MyConverter();

    public Converter getMyConverter(){
        return myConverter;
    }
}
```
#### Example to use custom converter
```xml
<label value="@load(vm.message) @converter(vm.myConverter)"/>

<label value="@load(vm.message) @converter('com.foo.MyConverter')"/>
```

Register Application Level Converters
=====================================
> Available for ZK: EE

> Since 6.0.1

You can register application level converters<sub>[1]</sub> by setting library-property(*org.zkoss.bind.appConverters*) in zk.xml.
```xml
<library-property>
    <name>org.zkoss.bind.appConverters</name>
    <value>foo=my.FooConverter,bar=my.BarConverter</value>
</library-property>
```
Then use them by converter name.
```xml
<label value="@load(vm.message) @converter('foo')"/>
<label value="@load(vm.message) @converter('bar')"/>
```

[1]: Application level converter only has one instance and is shared between all binders.

Use Built-in Converter
======================
ZK have provided some built-in application level converters that are commonly used. You can use them with name and parameters: **`@converter('converterName', parameterKey='value')`**.

Currently, built-in converter we provide are :
- **formattedNumber**
- **formattedDate**
```xml
    <label value="@load(item.price) @converter('formatedNumber', format='###,##0.00')"/>
    <label value="@load(item.creationDate) @converter('formatedDate', format='yyyy/MM/dd')"/>
```
- You should specify number or date pattern in format parameter's value for formattedNumber converter or formattedDate converter.

Use Converter in Children Binding
=================================
> since 6.0.1

Children-binding also supports converters, and it has a [default converter](/data_binding/children_binding.html) (If user doesn't assign one) to convert user provided object into a ` Collection ` object. Users can use a customized converter like other binding. If you implement a converter for children
binding, remember to return a ` Collection ` object in
` coerceToUi( ) `.

```xml
    <hlayout children="@init(vm.items) @converter(vm.itemConverter)">
        <template name="children">
            <label value="@load(each) "/>
        </template>
    </hlayout>
```
