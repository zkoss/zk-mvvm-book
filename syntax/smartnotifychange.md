# @SmartNotifyChange

Syntax
======
``` java
@SmartNotifyChange("anotherProperty")

@SmartNotifyChange({"secondProperty","thirdProperty"})

@SmartNotifyChange("*")

@SmartNotifyChange(".")
```

Description
===========
**Target:** method (command method)

**Purpose:** Notify value change once it changed, unlike `@NotifyChange`.
Usually the annotation is used with `@Command`.

Example
=======
``` java
public class OrderVM {

    //other code...

    //action command
    @SmartNotifyChange({"selected","orders","messages"})
    @Command
    public void newOrder(){
        Order order = new Order();
        getOrders().add(order); //add new order to order list
        selected = order;//select the new one
    }
}
```
