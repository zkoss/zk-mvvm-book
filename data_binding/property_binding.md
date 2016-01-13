# Property Binding

# Two Way Data Binding
Property binding makes developers bind any component's attribute to ViewModel's property and specify its access privilege. There are 3 kinds of access privilege: **save-load (`@bind`)**, **save only (`@save`)** and **load only(`@load`)**.

We usually use save only binding on input components, e.g., textbox, to collect user input into a JavaBean and load only binding when displaying data. If you have both needs, you can use save-load binding. We also call it **Non-conditional Property Binding**, because you cannot specify condition in this binding.

#### Save-load binding
```xml
<window id="window" title="maximize test" border="normal"
    maximizable="true" maximized="@bind(vm.maximized)">
</window>
```
- In 2 way (save-load) binding, when window's maximized attribute is changed, it will save value back to ViewModel's “maximized” property, and vice versa.

#### Save only binding
```xml
<textbox value="@save(vm.person.firstname)"/>
<textbox value="@save(vm.person.lastname)"/>
<intbox value="@save(vm.person.age)"/>
```
- In save-only binding, only textbox's value change will be saved to ViewModel. ViewModel's change won't be loaded to textbox.

#### Load only binding
```xml
<label value="@load(vm.welcomeMessage)"/>
```
- In load-only binding, only ViewModel's property change will be load to label.

The timing of saving a component attributes' value to ViewModel is **when the attribute related event fires**. For example, textbox's value is saved on the onChange event firing. After you finish your input in a textbox and move the cursor to next input field, the input data is saved to ViewModel at that time. The timing of loading is specified by ` @NotifyChange `.

If ViewModel's property is a map, the syntax is as follows:
```xml
<label value="@load(vm.myMapping['myKey'])"/>
```

Limitation
----------
#### Can't save to variable
In the following example, the value of **person.name** is loaded to textbox and after editing, the value is then saved to **person.name**.
```xml
<textbox value="@bind(person.name)"/>
```
However, if the expression only contains first-term<sub>[1]</sub>, 'load from' would still work, but 'save to' would not be allowed to perform. In the following example, the value **myname** is loaded to textbox, but after editing the value, EL exception occurs when saving value back.
```xml
<textbox value="@bind(myname)"/>
```
[1]: first-term is a variable that comes from a variable resolver in which its value cannot be saved by EL directly

Conditional Binding
===================
Sometimes we need to control the timing of saving or loading instead of depending upon the attribute related event firing. We can specify property ` before ` or ` after ` to control the timing upon ViewModel's Command. Therefore, only before or after executing specified Command, the attribute's value is saved (or loading) to a ViewModel.

Assume that there is an order management application with a database. The following zul example is one of its page, listbox displays order list and doublebox is for editing an order detail.

#### Save before a command
``` xml
<listbox model="@load(vm.orders)" selectedItem="@bind(vm.selected)" hflex="true" height="200px">
    <template name="model" var="item">
        <listitem >
            <listcell label="@load(item.id)"/>
            <listcell label="@load(item.quantity)"/>
            <listcell label="@load(item.price)"/>
        </listitem>
    </template>
</listbox>

<toolbar>
    <button label="New" onClick="@command('newOrder')"/>
    <button label="Save" onClick="@command('saveOrder')" disabled="@bind(empty vm.selected)"/>
    <!-- show confirm dialog when selected is persisted -->
    <button label="Delete" onClick="@command(empty vm.selected.id ? 'deleteOrder' : 'confirmDelete')"
        disabled="@load(empty vm.selected)"/>
</toolbar>

<grid hflex="true" >
    <columns>
        <column width="120px"/>
        <column/>
    </columns>
    <rows>
        <row>Quantity
            <intbox value="@load(vm.selected.quantity) 
                @save(vm.selected.quantity, before='saveOrder')"/>
        </row>
        <row>Price
            <doublebox value="@load(vm.selected.price) 
                @save(vm.selected.price, before='saveOrder')" format="###,##0.00"/>
        </row>
    </rows>
</grid>
```
- The listcell and doublebox are both bound to ` vm.selected.price `. (line 6, 31)
- If doublebox's value is saved immediately after user input, listcell's label which is also bound to the same ViewModel's property will also change. This effect might mislead the user that the value has been saved to database.
- To eliminate this misleading effect, developer might hope to batch save all editing result after the user click a “Save” button. We can achieve this by specifying Command's name for property ` before ` in ` @save `. The value of intbox and doublebox are batch-saved when the use clicks “Save” button. (line 27, 31)

Execution Order
===============
If a component has non-conditional property and command binding at the same time. The execution order is:
1.  Save binding
2.  Execute the command
3.  Load binding

#### A component with multiple binding
```xml
<textbox value="@bind(vm.filter)" onChange="@command('doSearch')"/>
```
- When onChange event fires, the binder saves vm.filter first, executes command “doSearch”, then loads vm.filter.

Multiple Conditions
-------------------
We also can specify multiple Command's name in an array of string literal for property ` before ` or ` after `like following:
#### Load after multiple commands
```xml
<label value="@load(vm.person.firstname, after={'update', 'delete'})"/>
```
When we use property binding to collect user input, we might need to validate it before saving to a ViewModel. ZK provides a standard validation mechanism through a reusable element called **validator**. We'll describe its detail [here](./validator.html).

Collection Binding
==================
We need collection binding when we bind a container component's “model” attribute of , e.g. listbox or grid, to a ViewModel. That target property of ViewModel must be a ` Collection ` object, e.g. ` List ` or ` Set `. When the getter method returns ` List `, binder wraps it as ListModel automatically. If getter already returns a ListModel object, binder won't wrap it.

We are using the following ViewModel to demonstrate usage of collection binding.
#### ViewModel for collection binding
```java
public class OrderVM {

    //the order list
    private List<Order> orders;

    //the selected order
    private Order selected;

    public List<Order> getOrders() {
        return orders;
    }

    public Order getSelected() {
        return selected;
    }
    public void setSelected(Order selected) {
        this.selected = selected;
    }
}
```

Iteration Variable
-----------------
### “var” Attribute
We usually use Collection Binding with `<template>` and specify its “var” attribute to name the **iteration variable** which represents each entry of the collection:
```xml
<template name="model" var="item">
    <!-- other components -->
</template>
```
If you don't specify iteration variable name in var, the default name of var is: **`each`**.

### Implicit Iteration Status Variable
ZK automatically set the variable name of iteration status upon the name of iteration variable and this variable stores the index of iteration. E.g. if you set ` var="item" `, then iteration index would be ` itemStatus.index `:
```xml
<listbox>
    <template name="model" var="item">
        <listitem >
            <listcell label="@load(itemStatus.index)"/>
        </listitem>
    </template>
</listbox>
```
If you don't specify iteration variable name in var, the default variable name of Implicit Iteration Status is: **`forEachStatus`**.

Binding on Listbox
------------------
#### Collection binding with listbox
```xml
<listbox model="@load(vm.orders)" selectedItem="@bind(vm.selected)" hflex="true" height="200px">
    <listhead>
        <listheader label="Row Index"/>
        <listheader label="Id"/>
        <listheader label="Quantity" align="center" width="80px"/>
        <listheader label="Price" align="center" width="80px"/>
        <listheader label="Creation Date" align="center" width="100px"/>
        <listheader label="Shipping Date" align="center" width="100px"/>
    </listhead>
    <template name="model" var="item">
        <listitem >
            <listcell label="@load(itemStatus.index)"/>
            <listcell label="@load(item.id)"/>
            <listcell label="@load(item.quantity)" style="@load(item.quantity lt 3 ? 'color:red' : '')"/>
            <listcell label="@load(item.price)"/>
            <listcell label="@load(item.creationDate)"/>
            <listcell label="@load(item.shippingDate)"/>
        </listitem>
    </template>
</listbox>
```
- We bind listbox's model to a Collection type property: ` List<Order> `. (line 1)
- We bind “seletedItem” to a ViewModel's property of same object type (` Order `) in a collection. When each time a user select an item of the listbox, binder will save the selected object to ViewModel. Therefore, we can get user's selection by this way. (line 1)
- The value of “var” attribute: item represents an object of the model, so use dot notation to reference its property like ` item.quantity `. Its index in the collection can be referenced by ` itemStatus.index `. (line 12)
- With power of EL, we can implement simple presentation logic on the ZUL. Here we display quantity number in red color when it's less than 3. (line 14)

#### Binding multiple selections
```xml
<listbox selectedItems="@bind(vm.selected)" model="@load(vm.model)">
    <!-- other components -->
</listbox>
```
#### ViewModel for multiple selections
```java
public class ListModelSelection {
    private List<String> model;
    private Set<String> selected;

    //getter and setter
}
```
- We can retrieve multiple selections by binding the attribute “selectedItems” to a Set.

Binding on Grid
---------------
The usage is similar for grid.
#### Collection binding usage for grid
```xml
<grid model="@load(vm.orders)">
    <columns>
        <column label="Id"/>
        <column label="Quantity"/>
        <column label="Price"/>
    </columns>
    <template name="model" var="item" >
        <row>
            <label value="@bind(item.id)"/>
            <label value="@bind(item.quantity)"/>
            <label value="@bind(item.price)"/>
        </row>
    </template>
</grid>
```

Dynamic Template
----------------
Dynamic template enables developers to specify **which template to apply upon different conditions**, e.g. grid. It is supported in grid, listbox, combobox, selectbox, and tree. You can use this feature with
`@template( [EL-expression] )`. The binder will evaluate EL expression as a template's name and look for corresponding template to render child components. If the specified template doesn't exist in current component, it looks up parent component's template. If no ` @template ` assigned, it uses template that named **model** by default.

#### Default case
```xml
<grid model="@bind(vm.orders)">
    <template name="model">
    <!-- child components -->
    </template>
</grid>
```
#### Template name specified.
```xml
<grid model="@bind(vm.orders) @template('myTemplate')">
    <template name="myTemplate">
    <!-- child components -->
    </template>
</grid>
```
You could use EL to decide which template to use.
#### Conditional
```xml
<grid model="@bind(vm.orders) @template(vm.type='foo' ? 'template1' : 'template2')">
    <template name="template1">
    <!-- child components -->
    </template>

    <template name="template2">
    <!-- child components -->
    </template>
</grid>
```

It also provides implicit variable: **each** (the instance of item in binding collection) and **forEachStatus** (the iteration status) when evaluating the template for each row. That is, you can request to use different `<template>` per each item in the binding collection.
```xml
<grid model="@bind(vm.orders) @template(each.type='A' ? 'templateA' : 'templateB')">
    <template name="templateA">
    <!-- child components -->
    </template>

    <template name="templateB">
    <!-- child components -->
    </template>

</grid>
```
- Assume that the object in binding collection has a property “type”. Its value could be A or B. (line 1)

Defferred Binding
================
In many cases, it would be sufficient to mark the onChange event deferrable to avoid unnecessary AU requests and fine-tune the application performance.
You can register component level deferPost by setting custom attributes(org.zkoss.bind.event.deferPost). By specifying this custom attribute to components, “saving” will occur during the next AU request.
```xml
    <textbox value="@bind(vm.text1)">
        <custom-attributes org.zkoss.bind.event.deferPost="false"/>
    </textbox>
    <textbox value="@bind(vm.text2)">
        <custom-attributes org.zkoss.bind.event.deferPost="true"/>
    </textbox>
    <button onClick='Clients.showNotification("AU request has sent")'>click me</button>
```
The value of the first textbox component would be saved when you change the input text(it's the same if you don't specify this custom attribute at all), but the value of the second textbox component would only be saved when an AU request is sent after the input text is changed, either by changing the first textbox's input, or clicking the button.
