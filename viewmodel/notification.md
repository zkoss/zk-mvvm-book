# Notification

# Overview
The binder is responsible for synchronizing data between the View and ViewModel. Typically the ViewModel is a POJO and has no reference to any UI component. Hence it cannot push data to a UI component. Developers have to specify the dependency of ViewModel's properties with ZK provided Java annotation, then binder knows when to reload which property and to update the UI view.

# Notify Change
ZK bind provides a set of Java annotations ( `@NotifyChange`, `@DependsOn`, `@NotifyChangeDisabled` ) to specify **when to reload which property to UI components**. Developers have to specify it at design time, and the binder will synchronize data at run-time. The binder keeps track of binding relationships between component's attribute and ViewModel's property. After each time it invokes a ViewModel's method (setter, getter, or command method), it looks for corresponding component attribute to reload according to specified properties in the annotation.

## Notify on Command
During execution of a command, one or more properties are changed because Command method usually contains business or presentation logic. Developers have to specify which property (or properties) is changed in Java annotation's element, then the data binding mechanism can reload them from ViewModel to View at run-time after executing the Command.

The syntax to notify property change:

One property:

**@NotifyChange("oneProperty")**

Multiple properties:

**@NotifyChange({"property01", "property02"})**

All properties in a ViewModel:

**@NotifyChange("*")**

Following is an example of `@NotifyChange`:
```java
public class OrderVM {

    //the order list
    List<Order> orders;
    //the selected order
    Order selected;

    //getter and setter

    @NotifyChange("selected")
    @Command
    public void saveOrder() {
        getService().save(selected);
    }

    /* In newOrder() , we change property "orders" and "selected",
    therefore we apply @NotifyChange to notify binder to reload 2 changed properties
    after command execution. */
    @NotifyChange({"orders", "selected"})
    @Command
    public void newOrder() {
        Order order = new Order();
        getOrders().add(order);
        selected = order;//select the new one
    }

    /* The deleteOrder() also change the same 2 properties.
    Assume that this ViewModel class has only 2 properties,
    we can use "*" (asterisk sign) to notify all properties in current class. */
    @NotifyChange("*")
    @Command
    public void deleteOrder() {
        getService().delete(selected);//delete selected
        getOrders().remove(selected);
        selected = null; //clean the selected
    }
}
```

## Notify on Setter
For similar reason, property might be changed in setter method. After value is saved to ViewModel's property, multiple components or other properties might depend on the changed property. We also have to specify this data dependency by `@NotifyChange`.

### Enabled by Default
A setter method usually changes only one property, e.g. setMessage() will set message to new value. To save developer's effort, **the target property changed by setter method is notified automatically**. That is, `@NotifyChange` annotation on a setter is not required unless there are some other properties changed by the setter and need to be explicitly notified.
```java
public class MessageViewModel {
    private String message;

    public String getMessage() {
        return this.message;
    }
    public void setMessage(String msg) {
        message = msg;
    }
}
```
* There is no need to specify `@NotifyChange` on `setMessage()` in MessageViewModel if only `message` is changed in the setter method.

The ZUL that uses MessageViewModel.
```xml
<textbox id="msgBox" value="@bind(vm.message)"/>

<label id="msg1" value="@load(vm.message)"/>

<label id="msg2" value="@load(vm.message)"/>
```
* When a user input value in msgBox and the value is saved back to ViewModel, the data binding mechanism will synchronize the value to msg1 and msg2 automatically.

Developer can disable this default behavior by adding `@NotifyChangeDisabled` on setter method.

### Specify Dependency
For setter method, `@NotifyChange` is used when multiple properties have dependency relationship, e.g. one property's value is calculated from the other properties. Notice that if you apply `@NotifyChange` on a setter method, default notification behavior will be overridden.

#### @NotifyChange on Setter
```java
public class FullnameViewModel {

    //getter and setter

    @NotifyChange("fullname")
    public void setFirstname(String firstname) {
        this.firstname = firstname;
    }

    /* By default setLastname() will notify "lastname" property's change.
    Becuase we apply NotifyChange on it, this default notification is overridden.
    We have to notify it explicitly. */
    @NotifyChange({"lastname", "fullname"})
    public void setLastname(String lastname) {
        this.lastname = lastname;
    }

    public String getFullname() {
        return (firstname == null ? "" : firstname)   + " "
            + (lastname == null ? "" : lastname);
    }
}
```
In above code snippet, fullname is concatenated by firstname and lastname. When firstname and lastname is changed by setter, the fullname should be reload to reflect its change.

If a property depends on many properties, `@NotifyChange` will distribute in multiple places in a ViewModel class. Developers can use `@DependsOn` for convenience. This annotation defines the dependency relations among properties and can provide same function as explicit `@NotifyChange`, but it's used on getter method.

#### Example of @DependsOn
```java
public class FullnameViewModel {

    //getter and setter

    public void setFirstname(String firstname) {
        this.firstname = firstname;
    }

    public void setLastname(String lastname) {
        this.lastname = lastname;
    }

    /* The example is functional equivalent to previous one, but written in
    reversed meaning. It means when any one of 2 properties (firstname, lastname) change,
    fullname should be reloaded. */
    @DependsOn({"firstname", "lastname"})
    public String getFullname() {
        return (firstname == null ? "" : firstname) + " "
                + (lastname == null ? "" : lastname);
    }
}
```

## Notify Bean Change
If you bind a component's attribute to a bean and want to reload it after the bean's property changed, you should apply the following syntax on the setter of that target property:
#### @NotifyChange(".")
Note a dot(.) is a bit different from an asterisk(\*) because an asterisk, `@NotifyChange("*")` , just notify binder to reload those components that bound to the bean's properties, e.g. `@bind(vm.person.firstname)`, `@bind(vm.person.lastname)`, and `@bind(vm.person.fullname)`. The component attribute that bound to the bean itself, i.e. `vm.person`, will **NOT** be reloaded with the `@NotifyChange("*")`. In such case you have to use `@NotifyChange(".")` to tell binder that the bean itself has changed and the component attribute bound to the bean shall be reloaded.

For what use cases you will need this? Most of the time, we bind one component attribute to only one bean property. However, there are cases that you might want to bind the component attribute to a calculation result of multiple bean properties; yet you don't want to put that calculation logic in the bean. Then you can bind the component attribute to the whole bean and have a converter to convert the data upon the bean's multiple properties.

For example, assume there is a web page in a tour website. A customer has to fill the leave and return day of a trip and the duration of the trip shall be automatically calculated and show on the screen after the user fills either fields. Here we choose to use a converter to do the duration calculation instead of implementing the logic in the Trip class as a property. In such scenario, you will need to annotate `@NotifyChange(".")` on both leaveDate and returnDate property setters so when either leaveDate or returnDate property changes, the duration attribute can be updated.
#### Bind to an object example
```xml
<hlayout>
    Leave Date: <datebox value="@save(vm.trip.leaveDate)"/>
</hlayout>
<hlayout>
    Return Date: <datebox value="@save(vm.trip.returnDate)"/>
</hlayout>
Duration including leave and return(day): 
    <label value="@load(vm.trip) @converter(vm.durationConverter)" />
```
* Because durationConverter needs to access 2 properties (leaveDate, returnDate), we have to bind label's value to the trip bean itself not an individual property of the bean.

#### @NotifyChange(".") example
```java
public class DurationViewModel  {
    private Trip trip = new Trip();

    public class Trip {
        private Date leaveDate;
        private Date returnDate;

        public Date getLeaveDate() {
            return leaveDate;
        }
        @NotifyChange(".")
        public void setLeaveDate(Date leaveDate) {
            this.leaveDate = leaveDate;
        }
        public Date getReturnDate() {
            return returnDate;
        }
        @NotifyChange(".")
        public void setReturnDate(Date returnDate) {
            this.returnDate = returnDate;
        }
    }
    public Converter getDurationConverter(){
        return new Converter() {
            public Object coerceToUi(Object val, Component component, BindContext ctx) {
                if (val instanceof Trip){
                    Trip trip = (Trip) val;
                    Date leaveDate = trip.getLeaveDate();
                    Date returnDate = trip.getReturnDate();
                    if (null != leaveDate && null != returnDate){
                        if (returnDate.compareTo(leaveDate) == 0){
                            return 1;

                        }else if (returnDate.compareTo(leaveDate) > 0) {
                            return ((returnDate.getTime() - leaveDate.getTime())/1000/60/60/24) + 1;
                        }
                        return null;
                    }
                }
                return null;
            }

            public Object coerceToBean(Object val, Component component, BindContext ctx) {
                return null;
            }
        };
    }
}
```
* About how to implement a converter, please refer to [Data Binding/Converter](../data_binding/converter.html).

## Notify Programmatically
Sometimes the changed properties we want to notify depends on value at run-time, so we cannot determine the property's name at design time. In this case, we can use [BindUtils.postNotifyChange(String queueName, String queueScope, Object bean, String property)](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/BindUtils.html#postNotifyChange(java.lang.String queueName, java.lang.String queueScope, java.lang.Object bean, java.lang.String property)) to notify change dynamically. The underlying mechanism for this notification is binder subscribed event queue that we talk about in [the binder section](../data_binding/binder.html). It uses **desktop scope** event queue by default.  
The Object used for the "bean" parameter can be any object used in a load binding. The "property" string refers to the specific property used in the binding. It is possible to target a bean that was used in a binding implicitely. For example, if collection is bound using forEach, the "each" variable will refer to each element of the collection, and can be notified individualy.  
Additionally, it is possible to notify an individual bean, instead of a bean property by using the "dot" syntax. (see example below)

#### Dynamic Notification
```java
String data;
// setter & getter

@Command
public void cmd() {
    if (data.equal("A")) {
        //other codes...
        BindUtils.postNotifyChange(null, null, this, "value1");
    } else {
        //other codes...
        BindUtils.postNotifyChange(null, null, this, "value2");
    }
    //notifying individual properties of an arbitrary object
    BindUtils.postNotifyChange(null, null, myBean, "value2");
    
    //notifying all properties of an arbitrary object
    BindUtils.postNotifyChange(null, null, myOtherBean, "*");
    
    //notifying an arbitrary object itself
    BindUtils.postNotifyChange(null, null, myOtherBean, ".");
}
```
* The first parameter of postNotifyChange() is queue name and second one is queue scope. You can just leave it null and default queue name ans scope (desktop) will be used.

We provide a simpler way to call postNotifyChange for the most common usage (omitting the queue name and scope) since 9.5.0.
You can pass the bean instance and one or more properties name of bean only.
For example:
```java
BindUtils.postNotifyChange(this, "propertyOne");
```
```java
BindUtils.postNotifyChange(this, "propertyOne", "propertyTwo", "propertyThree");
```









