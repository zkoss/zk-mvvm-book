# Binding Annotation for a Custom Component
When we apply data binding expression on a component's attribute, ZK automatically saves the value back and loads the value from the ViewModel. How does ZK know when to save or load which attribute of a component? It is declared in a ZK component's metainfo. ZK reads the component's metainfo and knows how to process data binding expressions on a component.

Before we declare data binding annotation for a new custom component, only load binding will work. We need to declare explicitly to let save binding work. This section describes about how to declare it.

Create a Custom Component
=========================

In order to explain how to declare data binding definition, we create a macro component with a custom class as an example. (Please [ refer here](http://books.zkoss.org/wiki/ZK_Developer%27s_Reference/UI_Composing/Macro_Component/Implement_Custom_Java_Class) for details.) The macro component is named "EditableLabel". It's a label with in-place editor. This component display a label first. When we double click it, it switch to a textbox for editing and change label's value.

``` java
public class EditableLabel extends HtmlMacroComponent {

    @Wire
    Textbox textbox;

    @Wire
    Label label;

    public EditableLabel() {
        setMacroURI("/WEB-INF/component/editablelabel.zul");
        // force the template to be applied, and to wire members automatically
        compose();
    }

    public String getValue() {
        return textbox.getValue();
    }

    public void setValue(String value) {
        textbox.setValue(value);
        label.setValue(value);
    }

    @Listen("onDoubleClick=#label")
    public void doEditing() {
        textbox.setVisible(true);
        label.setVisible(false);
        textbox.focus();
    }

    @Listen("onBlur=#textbox")
    public void doEdited() {
        label.setValue(textbox.getValue());
        textbox.setVisible(false);
        label.setVisible(true);
        Events.postEvent("onEdited", this, null);
    }
}
```

-   Line 34: This component sends custom event to notify that *Label*'s value is changed.

#### editablelabel.zul
``` xml
<zk>
    <label id="label" />
    <textbox id="textbox" visible="false"/>
</zk>
```

Declare Data Binding in Language Addon XML
==========================================

In order to let BindComposer knows how to process a custom component's data binding expression, we should declare data binding definition. For complete XML's element and attributes, please refer to [ Data Binding](http://books.zkoss.org/wiki/ZK_Component_Reference/Annotation/Data_Binding) and [ Language Definition](http://books.zkoss.org/wiki/ZK_Client-side_Reference/Language_Definition). Here we only cover mostly commonly used attributes.

For our example, the key point is to declare when to save component's "value" property to a ViewModel's member variable. The saving time is specified by **event name**.

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<language-addon>
    <addon-name>myaddon</addon-name>
    <language-name>xul/html</language-name>
    <component>
        <component-name>editableLabel</component-name>
        <component-class>org.zkoss.reference.developer.mvvm.advance.EditableLabel</component-class>
        <annotation>
            <annotation-name>ZKBIND</annotation-name>
            <property-name>value</property-name>
            <attribute>
                <attribute-name>ACCESS</attribute-name>
                <attribute-value>both</attribute-value>
            </attribute>
            <attribute>
                <attribute-name>SAVE_EVENT</attribute-name>
                <attribute-value>onEdited</attribute-value>
            </attribute>
        </annotation>
    </component>
</language-addon>
```

-   Line 3: You should give a unique name.
-   Line 6-7: These two elements are required.
-   Line 10: Specify which property of this component that ZK should save.
-   Line 16-17: Specify the event name that when the component sends it, ZK should save the specified property.

Declare Data Binding by Java Annotation
=======================================

We can also declare data binding definition by Java annotation, [ComponentAnnotation](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/annotation/ComponentAnnotation.html). It's an alternative to XML file. In order to make the custom component be available for all pages, we still need to declare it in lang-addon.xml which we mentioned above.

``` xml
<language-addon>
    <addon-name>myaddon</addon-name>
    <language-name>xul/html</language-name>
    <component>
        <component-name>editableLabel</component-name>
        <component-class>org.zkoss.reference.developer.mvvm.advance.EditableLabel</component-class>
    </component>
</language-addon>
```

The following annotation has the same effect as the XML file of previous section.

``` java
@ComponentAnnotation("value:@ZKBIND(ACCESS=both, SAVE_EVENT=onEdited)")
public class EditableLabel extends HtmlMacroComponent {
    //class body
}
```

We can apply `@ComponentAnnotation` on two targets; one is **getter (or setter) method** and another is **class**. This annotation has only one element whose type is String.

Applying the annotation on a getter means we annotated on the property that the getter method gets.For example, you could apply on "value" property as follows:

``` java
@ComponentAnnotation("@ZKBIND(ACCESS=both, SAVE_EVENT=onChange)")
public String getValue() {
    // method body
}
```

The syntax of annotation's element is the same as [ZUML's annotations](http://books.zkoss.org/wiki/ZK_Developer%27s_Reference/Annotations/Annotate_in_ZUML).

**`@ZKBIND( [ATTRIBUTE_NAME1]=[ATTRIBUTE_VALUE1], ...)`**

If the component's Java class doesn't have the getter or setter for the given property, you can specify the annotations at the class level by prefixing the annotation with the **property name** and a **colon**. For example,

``` java
@ComponentAnnotation({"selectedItem: @ZKBIND(ACCESS=both, SAVE_EVENT=onSelect)",
    "openedItem: @ZKBIND(ACCESS=load, LOAD_EVENT=onOpen)")
public class Foo extends AbstractComponent {
    // class body
}
```
