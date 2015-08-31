# @NotifyChange

Syntax
======
``` java
@NotifyChange("anotherProperty")

@NotifyChange({"firstProperty", "secondProperty"})

@NotifyChange("*")

@NotifyChange(".")
```

Description
===========
**Target:** method (setter or command method)

**Purpose:** To notify binder one or more properties change.

By default, a property set by binder through setter method will notify this property changed without this annotation. You could use this annotation on setter method to override default notification target. You could also add this annotation on a command method to notify properties that are changed after command execution. To avoid the default notification, use ` @NotifyChangeDisabled ` on setter method solely. Giving "*" in annotation element means notify all properties in a ViewModel.

Use "." to enforce reloading the instance of the class in where the annotation locates, not an instance's property.

Example
=======
``` java
public class OrderVM {

    //other code...

    @NotifyChange({"selected", "messages"})
    public void setSelected(Order selected) {
        this.selected = selected;
    }

    //action command
    @NotifyChange({"selected", "orders", "messages"})
    @Command
    public void newOrder() {
        Order order = new Order();
        getOrders().add(order); // add new order to order list
        selected = order; // select the new one
    }
}
```
