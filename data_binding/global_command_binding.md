# Global Command Binding

Overview
========
**Global Command Binding** is similar to command binding but the target becomes a global command. The local command can only be triggered by events of a ViewModel's Root View Component and its child components. The main difference from command binding is the event doesn't have to belong to the ViewModel's root view component or its child component. **By default we can bind an event to any ViewModel's global command within the same desktop**. When we trigger a global command, all matched global command in all ViewModels of the same scope will be executed.

![MVVM Global Command Overview](http://books.zkoss.org/images/6/6a/Mvvm-global-command-overview.png)

Default Global Command
----------------------
> Since 6.5.1

Global command also supports **`@DefaultGlobalCommand`** to mark a method as a default global command method which is invoked only if a global command binding doesn't match any other global command methods. When a binder receives a global command, it starts to find ViewModel's command methods by matching its name. Only if the binder cannot find a matched method, it invokes default global command method.

Assume that there are only two global command methods in the below
ViewModel. If we trigger a global command "exit", a binder invokes the
default global command method `defaultAction()` because it cannot find a
command method named "exit".

#### ViewModel with default command method
```java
public class OrderVM {

    @GlobalCommand
    public void newOrder(){
    ...
    }

    @DefaultGlobalCommand
    public void defaultAction(){
    ...
    }
}
```

Usage
=====

Notification
------------
Assume we have 2 areas in a page, one is main area for adding items and another is list area for displaying items. Each area is bound to a ViewModel. Two ViewModels can access the same item list from single data source, for simplification we use a static list as data source. The main issue is how to notify another ViewModel to refresh item list after we click the "Add" button (add a new item). We can achieve it by binding "Add" button's onClick event to a global command of list area's ViewModel and in that global command we refresh the item list and notify related properties change to update View.

The interface looks like this:

![MVVM Global Command Simple](http://books.zkoss.org/images/6/6a/Mvvm-global-command-simple.png)

#### A zul with 2 ViewModels
```xml
        <hlayout>
            <vbox id="mainArea" width="200px" height="300px"
                style="border:dashed 2px" visible="@bind(vm.visible)"
                apply="org.zkoss.bind.BindComposer"
                viewModel="@id('vm') @init('org.zkoss.mvvm.examples.globalcommand.AddViewModel')"
                validationMessages="@id('vmsgs')">
                <label value="Main Panel" style="font-size:30px" />

                Enter a Item (at least 3 characters):
                <textbox id="iBox"
                    value="@load(vm.item)@save(vm.item, before='add') @validator(vm.itemValidator)" />
                <label value="@load(vmsgs[iBox])" style="color:red" />
                <button label="Add"
                    onClick="@command('add') @global-command('refresh')" />
                <separator height="20px" />
                <label value="@load(vm.msg)" />
            </vbox>

            <vbox id="listArea" width="400px" height="300px"
                visible="@bind(vm.visible)" apply="org.zkoss.bind.BindComposer"
                style="border:solid 2px"
                viewModel="@id('vm') @init('org.zkoss.mvvm.examples.globalcommand.ListViewModel')">
                <listbox model="@load(vm.items)">
                    <listhead>
                        <listheader label="Items"/>
                    </listhead>
                    <template name="model">
                        <listitem>
                            <listcell label="@load(each)"/>
                        </listitem>
                    </template>
                </listbox>
                <label value="@load(vm.lastUpdate)" />
            </vbox>
        </hlayout>
```
-   We bind onClick event to a local command "add" and a global command "refresh".

#### AddViewModel.java
```java
public class MainViewModel {

    private String msg;

    @Command @NotifyChange("msg")
    public void add(){
        ItemList.add(item);     //add an item
        msg = "Added "+item;    //update message
    }
    //other code
}
```

#### ListViewModel.java
```java
public class ListViewModel {

    private List<String> items;
    private Date lastUpdate;

    @GlobalCommand @NotifyChange({"items","lastUpdate"})
    public void refresh(){
        items = ItemList.getList();
        lastUpdate = Calendar.getInstance().getTime();
    }
    //other code
}
```
-   Declare a global command and notify the change. (line 6)
-   The command method reloads items and set current time as last update time. (line 7)

#### A new item "hat" is added

![MVVM Global Command Simple Add](http://books.zkoss.org/images/2/24/Mvvm-global-command-simple-add.png)

In above example, we bind onClick event to local and global commands, the global command will always be executed after local command is executed. If the local command is not executed for validation failure, the global command will not be executed. We have a validator to validate item's name, its length can not be less than 3. When we enter a short item name to violate the validation rule, the local command is not executed. So, the global command is not executed, either. We can confirm this behaviour because the message in main area and last update in list area doesn't change.

**Validation failure blocks command execution**

![MVVM Global Command Simple Fail](http://books.zkoss.org/images/4/4d/Mvvm-global-command-simple-fail.png)

One to Many Communication
-------------------------
We can also use global command binding to communicate multiple ViewModels at one event firing. If an event bound to a global command named "show", all global commands with name "who" in all ViewModels will be executed. Another difference from local command is that you can bind an event to a global command without ensuring its existence. If the global command you bound doesn't exist, it has no response and doesn't throw any exception. The relationship between event and global command is like **publisher-subscriber**. Publisher publishes an event, only those ViewModels who subscribes it execute the corresponding global command.

Based on previous example, assume that we want to hide and show 2 areas simultaneously with menuitems. We can bind a menuitem's onClick event to a global command named "show', and 2 ViewModels implements the "show" command to show itself. So does "hide" command.

The interface is:

![MVVM Global Command Menu](http://books.zkoss.org/images/5/5a/Mvvm-global-command-menu.png)

#### One to many communication
```xml
    <vlayout >
        <menubar width="600px" apply="org.zkoss.bind.BindComposer"
        viewModel="@id('vm') @init('org.zkoss.mvvm.examples.globalcommand.ControlViewModel')">
            <menuitem label="Show" onClick="@global-command('show')"></menuitem>
            <menuitem label="Hide" onClick="@global-command('hide')"></menuitem>
        </menubar>
        <!-- main area-->
        <!-- list area-->
    </vlayout>
```
-   Add a menubar with 2 menuitem: show and hide that are bound to global commands.

#### ViewModel implement show & hide command
```java
public class MainViewModel {

    private boolean visible = true;

    @GlobalCommand @NotifyChange("visible")
    public void show(){
        visible = true;
    }
    @GlobalCommand @NotifyChange("visible")
    public void hide(){
        visible =false;
    }
}
```
-   ListViewModel's implementation should have identical command methods.

When clicking hide menuitem, both ViewModel's global command "hide" will be executed thus 2 areas disappear.

Command Execution
=================
Global command's execution is quite simple. It just executes a command method and then reloads those properties that specified in ` @NotifyChange ` annotation. But if you bind an event to a local and global command at the same time, binder always **executes local command first**. If any reason blocks the local command's execution, e.g. validation fails, it  won't execute the global command.

![MVVM Global Command Execution](http://books.zkoss.org/images/1/18/Mvvm-global-command-execution.png)

Background Concept
==================
When we apply a [BindComposer](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/bind/BindComposer.html) to an component, it automatically creates a binder for us. As the ViewModel is a POJO, binder is like a broker to communicate with others. By default all binders subscribe to a default desktop-scoped event queue, thus this is a common communication mechanism among binders.

When we trigger a global command by an event, a binder posts an event to the queue it subscribes. All binders (including the binder that posts the event) that subscribe to the same queue in the same scope will receive this event and try to look up requested global command in their associated ViewModel. If a binder finds a matched global command, it will execute it. Otherwise, it ignores the event without throwing any exception.

![MVVM Binder](http://books.zkoss.org/images/7/7b/Mvvm-binder.png)

In above image, when we trigger a global command "show" in View A, binder A posts an event to the event queue. Binder B which subscribes to the same event queue and binder A itself will both receive the event and try to look up for global command "show" in their associated ViewMoels (ViewModel A and ViewModel B)

ZK allows you to change the name and scope of the event queue a binder subscribes to. Please refer to [ZK Developer's Reference/MVVM/DataBinding/Binder]().
#link not yet

Trigger a Command Dynamically
=============================
Except triggering a global command in a ZUL, we can also do it by calling API. For above example, we can trigger global command in the local command "add" and the code is as follows:
```java

    @Command @NotifyChange("msg")
    public void add(){
        ItemList.add(item);     //add an item
        msg = "Added "+item;    //update message
        BindUtils.postGlobalCommand(null, null, "refresh", null);
    }
```
-   The first parameter of BindUtils.postGlobalCommand() is queue name, and the second one is queue scope.

You can call this method in a composer, and those pages written in MVC pattern can communicate with ViewModel. A sample code snippet is as follows:
```java
public class MyComposer extends SelectorComposer{

    //other codes

    @Listen("onClick=button#postx")
    public void postX(){
        Map<String,Object> args = new HashMap<String,Object>();
        args.put("data", "postX");
        BindUtils.postGlobalCommand("myqueue", EventQueues.DESKTOP, "cmdX", args);
    }

}
```
