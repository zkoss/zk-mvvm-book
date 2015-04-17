# Communication between ViewModel and Composer
To get an in-depth understanding of the following paragraphs, it is recommended that you have read and understood the [ concept of global command](../viewmodel/commands.html#global-command) and [ global command binding](../data_binding/global_command_binding.html) prior reading this article.

The [ global command binding usage section](../data_binding/global_command_binding.html#usage) demonstrates how you can communicate between multiple ViewModels. We can also use the same mechanism to perform communication between a composer and a ViewModel, but of course, usage is a little different.

Posting a Command from a Composer to a ViewModel
================================================

We know that global command is triggered by [ binder sending events into event queue](../data_binding/binder.html), hence, ViewModels attached with binders are able to communicate with each other by global command. However, a composer doesn't have a binder to send a global command, therefore, we provide a utility class, **[BindUtils](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/BindUtils.html)**, that can do this job. You can use it in a composer.

For example, after adding a product, you want to tell ` ShoppingCartViewModel ` to refresh shopping cart's items. Assuming that we do not change the default setting such that ShoppingCartViewModel's binder (receiver) subscribes to the default event queue.

#### Send a global command in a composer (Sender)
``` java
public class MyComposer extends SelectorComposer{

    @Listen("onAddProductOrder=#PrdoDiv #prodGrid row productOrder")
    public void addProduct(Event fe) {

        //business logic of adding a product

        BindUtils.postGlobalCommand(null, null, "updateShoppingCart", null);
    }
}
```
-   We use ` BindUtils.postGlobalCommand(String queueName, String queueScope, String cmdName, Map<java.lang.String,java.lang.Object> args) ` to post a command. We leave first two arguments as "null" to use default queue name and default scope (**desktop**). The third arguments is **global command's name**. You can send extra parameters with a Map by the fourth argument.
-   The specified global command is then executed by all ViewModel's binders that have subscribed to the event queue in the same (desktop) scope.

In the <b>ShoppingCartViewModel</b>, we should declare a global command method named "updateShoppingCart" to receive this command request and refresh cart items. The code snippet below shows this.

#### Global command in a ViewModel (Receiver)
``` java
public class ShoppingCartViewModel {

    ...

    @GlobalCommand
    @NotifyChange("cartItems")
    public void updateShoppingCart() {
        //update shopping cart
    }
}
```
-   As [a binder subscribes to desktop scope event queue by default](../data_binding/binder.html), we only need to declare a global command.
-   To receive parameters with global command, please refer to [Advanced/Parameters\#A\_Global\_Command\_Example](./parameters.html#a-global-command-example).

Posting a Command from a ViewModel to a Composer
================================================
As a ViewModel has a binder attached to it, triggering a global command doesn't need [BindUtils](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/BindUtils.html), we can simply use global command binding.

Assuming that we want to inform a composer to update shopping cart's items.

#### Bind global command in a ZUL (Sender)
``` xml
 <window apply="org.zkoss.bind.BindComposer" binder="@init(queueName='myqueue')"
   viewModel="@id('vm') @init('example.MyViewModel')" >

    <button id="addProduct" label="Add" onClick="@global-command('updateShoppingCart')"/>
</window>
```
-   We set the queue name as 'myqueue' in which the binder publishes to.

As mentioned earlier, global command is sent by event queue, the composer (receiver) should subscribe to the same scope event queue to receive this global command.

#### To subscribe global command (Receiver) to the event queue
``` java
public class MyComposesr extends SelectorComposer<Component>{

    @Subscribe("myqueue")
    public void updateShoppingCart(Event evt){
        if(evt instanceof GlobalCommandEvent){
            if("updateShoppingCart".equals(((GlobalCommandEvent)evt).getCommand())){
                //update shopping cart's items
            }
        }
    }
}
```
-   Subscribe to a queue named 'myqueue' because previous binder publishes to this queue.
-   For ` @Subscribe `, please refer to [ZK Developer's Reference/MVC/Controller/Subscribe to EventQueues](http://books.zkoss.org/wiki/ZK_Developer%27s_Reference/MVC/Controller/Subscribe_to_EventQueues).
-   To subscribe an event queue by method call, please refer to [ZK Developer's Reference/Event Handling/Event Queues\#Subscribe\_to\_an\_Event\_Queue](http://books.zkoss.org/wiki/ZK_Developer%27s_Reference/Event_Handling/Event_Queues#Subscribe_to_an_Event_Queue)

