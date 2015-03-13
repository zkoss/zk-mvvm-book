# Commands

# Introduction
The ViewModel is an abstraction of the View. The View is responsible for displaying information and interacting with users. The information corresponds to ViewModel's property and interaction corresponds to ViewModel's **Command**. The **Command** is an action to manipulate ViewModel's property. Each command provides an action that the View can perform on ViewModel. These actions are also the ways that users can interact with View. For example, a ViewModel provides 2 commands: "save" and "delete". It means users can only perform these 2 actions on the View with the ViewModel. They could perform actions through clicking buttons or menuitems.

As ViewModel acts a role like Controller, developers can bind a UI component's event to a Command by specifying Command's name which is similar to register a event listener. Multiple events can bind to the same Command. When a user interacts with a component (e.g. click a button), the component fire an event then the data binding mechanism triggers the execution of Command. The Command may modify ViewModel's properties and then information displayed on View changes.

![MVVM ViewModel Command](http://books.zkoss.org/images/d/db/Mvvm-viewmodel-command.png)

The Command is implemented as ViewModel's method. Because ViewModel is a POJO, in order to make data binding mechanism identify which method represent a Command, developers have to annotate the method with ZK provided `@Command` annotation. We'll use the term: **Command method** to depict the special annotated method of a ViewModel in the later section. These methods usually manipulate ViewModel's property, like deleting an item. Firing a component's event triggers the execution of bound command, that is invoking the Command method. During executing the Command, the developer also has to specify what properties change to notify through Java annotation that we will describe in [later section](/syntax/notifychange.html)

#Declare Commands

## Local Command
ViewModel's Command is like an event handler, we can bind an event to a Command. The binding between events and a command is what we call "Event-Command Binding". Before establish this binding, we have to declare a Command with its name in a ViewModel. Be careful that command names in a ViewModel cannot be duplicated, or it will cause run-time exception.
```java
public class OrderVM {

    // create and add a new order to a list
    // command name is set to method name by default
    /* Notice that we can declare a Command without specifying its name,
    and its name is set to method name by default. */
    @Command
    public void newOrder(){
        Order order = new Order();
        getOrders().add(order); //add a new order to order list
        selected = order;//select the new one
    }

    // save an order
    // command name is specified
    /* We can also give Command's name by @Command('userDefinedName'). */
    @Command("save")
    public void saveOrder(){
        orderService.save(selected);
    }

    // delete an order
    // multiple command names
    /* We can even give multiple Command's name with array of String. */
    @Command({"delete", "deleteOrder"})
    public void deleteOrder(){
        //delete order
    }
}
```
Then we can bind component's event to the command in the ZUL.
```xml
<toolbar>
    <button label="New" onClick="@command('newOrder')" />
    <button label="Save" onClick="@command('save')" />
</toolbar>
```
We describe the detail of [command binding here](/data_binding/command_binding.html). This binding allow you to pass parameters to Command method, please refer [here](/advanced/parameters.html).

## Global Command
Global Command is also a ViewModel's command and can hook UI component's events to it. The local command can only be triggered by events of a ViewModel's Root View Component and its child components. The global command can be triggered by a component's event from any ZUL. The main difference of a global command from local command is that the event doesn't have to belong to the ViewModel's root view component or its child component. By default we can bind an event to any ViewModel's global command **within the same desktop**. A method can be both a local command and a global command.
```java
@Command("delete") @GlobalCommand("delete")
public void deleteOrder(){
    //...
}
```
We can declare multiple global commands with same name in different ViewModel. When an event triggers a global command, all matched command methods in every ViewModel will be executed.
```java
public class MainViewModel {

    @GlobalCommand
    public void show(){
        //...
    }
}


public class ListViewModel {

    @GlobalCommand
    public void show(){
        //...
    }
}
```
* If we trigger global command "show", each binder associated with each ViewModel will execute the show global command method but not in any particular order.

# Command Execution
A command execution is a mechanism of ZK Bind where it performs a method call on the ViewModel. It binds to a component's event and when a binding event comes, binder will follow the lifecycle to complete the execution. We'll describe this in detail in [Command Binding](/data_binding/command_binding.html) and [Global Command Binding](/data_binding/global_command_binding.html).
