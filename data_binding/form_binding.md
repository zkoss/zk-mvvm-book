# Form Binding

Overview
========
Form binding is like a buffer. It automatically creates a middle object. Before saving to ViewModel all input data is saved to the middle object. In this way we can keep dirty data from saving into the ViewModel before
user confirmation. Assuming a user fills a form in a web application, user input data is directly saved to ViewModel's properties, the target object. Then the user cancel the filling action before submitting the
form, thus the data stored in ViewModel is deprecated. That would cause trouble if we further process the deprecated data, so developers might store input data in a middle place first then move to real target object
after the user confirms it. Form binding provide a middle object to store unconfirmed input without implementing by yourself.

Form binding can keep target object in ViewModel unchanged before executing a Command for confirmation. Before saving to ViewModel's properties (target object) upon a Command, we can save input in Form binding's middle object. When the command is executed (e.g. button is clicked), input data is really saved to ViewModel's properties. Developers can achieve the whole process easily just by writing ZK bind expression and it reduces developer's burden of cleaning dirty data manually or implementing buffer himself.

#### The data flow among ZUL, middle object, and the target object is illustrated below:
![MVVM Form Binding](http://books.zkoss.org/images/c/cb/Mvvm-form-binding.png)


Steps
-----
1. Give an id to middle object in form attribute with ` @id `.
    * You can reference the middle object in ZK bind expression with id.
2. Specify ViewModel's property to be loaded with ` @load `
3. Specify ViewModel's property to save and before which Command with ` @save `
    * This means binder will save middle object's properties to ViewModel before a Command
4. Bind component's attribute to middle object's properties like you do in property binding.
    * You should use middle object's id specified in ` @id ` to reference its property.

An example using property binding :
```xml
<groupbox >
    <grid hflex="true" >
        <columns>
            <column width="120px"/>
            <column/>
        </columns>
        <rows>
            <row>Id
            <hlayout>
            <label value="@load(vm.selected.id)"/>
            </hlayout> </row>
            <row>Description <textbox value="@bind(vm.selected.description)"/></row>
            <row>Quantity
                <intbox value="@bind(vm.selected.quantity)"/>
            </row>
            <row>Price
                <doublebox value="@bind(vm.selected.price)" format="###,##0.00" />
            </row>
        </rows>
    </grid>
</groupbox>
```
- The input data is directly saved to properties of vm.selected. No middle object.

The same example but using form binding:
```xml
<groupbox form="@id('fx') @load(vm.selected) @save(vm.selected, before='saveOrder')">
    <grid hflex="true" >
        <columns>
            <column/>
            <column/>
        </columns>
        <rows>
            <row>Id
            <hlayout>
            <label value="@load(fx.id)"/>
            </hlayout> </row>
            <row>Description <textbox value="@bind(fx.description)"/></row>
            <row>Quantity
                <intbox value="@bind(fx.quantity) "/>
            </row>
            <row>Price
                <doublebox value="@bind(fx.price)" format="###,##0.00" />
            </row>
        </rows>
    </grid>
</groupbox>
```
- We give an id: ` fx ` to the middle object. (line 1)
- The properties of `fx` we can access are identical to vm.selected. (line 12,14,17)
- Binder saves input data into `fx`, middle object, when each time a user triggers an onChange event. When clicking a button bound to Command 'saveOrder', it will save middle object's data to `vm.selected`.

Middle Object
-------------
Form binding automatically creates a middle object for you to store properties from ViewModel's object you specified. But it **only stores those properties which attributes are bound to**. If you still need to access a property that are not stored in the middle object, you can access it from ViewModel's original object. Assume that ` vm.selected ` has 4 properties: **id, description, quantity, price**. For the example below:
```xml
<groupbox form="@id('fx') @load(vm.selected) @save(vm.selected, before='saveOrder')">
    <grid hflex="true" >
        <rows>
            <row>Id <label value="@load(fx.id)"/>
            </row>
            <row>Description <textbox value="@bind(fx.description)"/>
            </row>
            <row>Quantity <intbox value="@bind(fx.quantity) "/>
            </row>
        </rows>
    </grid>
</groupbox>
```
- Because we only bind attributes to 3 properties:“id”, “description”, and “quantity”, the middle object only stores these 3 properties. Therefore the “price” property is not stored in middle object, we cannot reference it without binding it first. For example, we cannot pass it as parameter by ` @command('cmd', currentPrice=fx.price) `, because ` fx.price ` doesn't exist in the middle object.

Form Status Variable
====================
Form binding also records middle object's modification status.It’s a common requirement for users to know that whether they have modified a form’s data (dirty status) or not, developers therefore add a feature that would remind users of this with an UI effect. For example, some text editors appends a star symbol ‘*’ on the title bar to remind users of modified text file. “Form binding” preserves the dirty status in an variable that we can utilize it.

Dirty status is stored in an auto-created **form status variable** with a naming convention of:

`[middleObjectId]Status`

Continue above example, we add an exclamation icon right next to Id value. If users modify any input data, the ex exclamation icon will show up.
```xml
        <groupbox form="@id('fx') @load(vm.selected) @save(vm.selected, before='saveOrder')" >
            <grid hflex="true" >
                <columns>
                    <column width="120px"/>
                    <column/>
                </columns>
                <rows>
                    <row>
                        Id
                        <hlayout>
                            <label value="@load(fx.id)" />
                            <image src="@load(fxStatus.dirty?'exclamation.png':'')" />
                        </hlayout>
                    </row>
                    <row>Description <textbox value="@bind(fx.description)"/></row>
                    <row>Quantity
                        <intbox value="@bind(fx.quantity) "/>
                    </row>
                    <row>Price
                        <doublebox value="@bind(fx.price)" format="###,##0.00" />
                    </row>
                    <row>Total Price
                        <label value="@load(fx.totalPrice)" />
                    </row>
                    <row>Creation Date
                        <datebox value="@bind(fx.creationDate)"/>
                    </row>
                    <row>Shipping Date
                        <datebox value="@bind(fx.shippingDate)"/>
                    </row>
                </rows>
            </grid>
        </groupbox>
```
-   In this example, form status variable is `fxStatus ` for the form’s id is ` fx `. Its **dirty** property indicates that whether the form has been modified by users or not.

![MVVM FormBinding Form Dirty](http://books.zkoss.org/images/5/54/Smalltalks-mvvm-in-zk6-formbinding-form-dirty.png)

After users modify a field, an exclamation icon shows up next to “Id” field. If users click “Save” button or change data back to original value, the exclamation icon disappears.

Initialize with Form Object
===========================
If you want to gain more control over form binding, e.g. to manipulate the middle object, you can provide an object which implements [Form](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/Form.html) interface (or you can use [SimpleForm](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/SimpleForm.html) for convenient) within ` @init() `.
This will initialize form binding with your own middle object (Form object), then you can do whatever you want like notifying others when middle object's properties change.

#### Initialize with Form object
```xml
<groupbox form="@id('fx') @init(vm.myForm) @load(vm.selected) @save(vm.selected, before='saveOrder') " >
```

#### Prepare a Form object
```java
public class OrderVM {
    Form myForm = new SimpleForm();

    public Form getMyForm() {
        return myForm;
    }
}
```

Form Validation
===============
Before saving data to form's middle object, we also can validate user input with validator. Please refer to [ZK Developer's Reference/MVVM/DataBinding/Validator]().
#link not yet
