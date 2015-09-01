# Data and Collections

# Expose ViewModel's Properties
ViewModel, like a JavaBean, exposes its properties through getter and setter methods. Any property that View (ZUL) wants to retrieve through data binding should have a corresponding getter method. The method's return type can be primitive type (int, boolean...), or a JavaBean. If a UI component needs to save its attribute value (it's usually user input) back to ViewModel's property, the ViewModel should provide a corresponding setter method. Therefore, through setter and getter can change the data of both the View and the ViewModel using the data binding mechanism.

## Property is Primitive Type

### Primitive type property
```java
public class PrimitiveViewModel {

    private int index;
    private double price;

    public int getIndex() {
        return index;
    }
    public void setIndex(int index) {
        this.index = index;
    }

    public double getPrice() {
        return price;
    }
    public void setPrice(double price) {
        this.price = price;
    }

}
```
### A zul that uses PrimitiveViewModel
```xml
<window apply="org.zkoss.bind.BindComposer" viewModel="@id('vm') @init('foo.PrimitiveViewModel')">

    <intbox value="@bind(vm.index)"/>

    <doublebox value="@bind(vm.price)"/>

    <!-- other components -->

</window>
```
* `@id('vm')` sets ViewModel name, then we can use vm to reference ViewModel's property in the following components. (line 1)

## Property is Object or JavaBean
If a property is a JavaBean, that JavaBean's property can also be accessed through an EL expression.

### Object type property
```java
public class Address {

    private String city;
    private String street;

    //gettter & setter
}

public class ObjectViewModel {

    private Integer index;
    private String name;
    //JavaBean
    private Address address;

    public int getIndex() {
        return index;
    }
    public void setIndex(Integer index) {
        this.index = index;
    }

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    public Item getAddress() {
        return address;
    }
    public void setAddress(Address address) {
        this.address = address;
    }

}
```

### A zul that uses ObjectViewModel.
```xml
<intbox value="@bind(vm.index)"/>

<textbox value="@load(vm.name)"/>

<label value="@load(vm.address.street)"/>
```
* `vm.address` is a JavaBean, and we can access its property **street** with EL expression `vm.address.street`. (line 5)

## Property is Collection
If the UI component is a collection container like listbox or grid, it should be bound to a property whose type is `Map` or subinterface of `Collection` like `List` or `Set`.

### Collection type property
```java
public class CollectionViewModel {
    //primitive type
    private int selectedIndex;
    //Collection
    private List itemList;

    public int getSelectedIndex() {
        return selectedIndex;
    }
    public void setSelectedIndex(int index) {
        selectedIndex = index;
    }

    /* As we can't save whole list through data binding,
    the ViewModel only provides getter for itemList.*/
    public List getItemList() {
        return itemList;
    }
}
```

### A zul that uses CollectionViewModel
```xml
<label value="@load(vm.address.street)"/>
<listbox model="@load(vm.itemList)" selectedIndex="@bind(vm.selectedIndex)">
    <template name="model" var="item">
        <listitem label="@load(item.name)"/>
    </template>
</listbox>
```
* Line 2: Listbox's `model` attribute should be bound to a collection object, `vm.itemList`.
