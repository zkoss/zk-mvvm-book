# @AutoNotifyChange
> Available for ZK: EE

> Since 8.5.1

Syntax
======
``` java
@AutoNotifyChange
```

Description
===========
**Target:** class (view model)

**Purpose:** Enable posting NotifyChange when setter method called.

If the view model class is annotated with this annotation, every setter method in `@Command` and `@GlobalCommand` will post NotifyChange after being called.

Example
=======
``` java
@AutoNotifyChange
public class OrderVM {

    //other code...

    //action command
    //no need to declare NotifyChange explicitly
    @Command
    public void newOrder() {
        Order order = new Order();
        getOrders().add(order);
        setSelected(order); // Ensure to use a setter method
        setOrders(getOrders()); // A workaround to notify the orders
    }
}
```
