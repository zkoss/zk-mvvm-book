# Client Binding


> Since 8.0.0

To interact with client-side libaries, **Client Binding** can help us publish a ZK’s data binding command on a native html element.
For example, you can publish a *doClick* command in your view model with an *onClick* event in a html Button.

### Implementation

**Client Binding** provides 4 methods - 2 on the client-side, and 2 on the server-side. Their relationships can be illustrated by the following diagram:

![ZK8_Client_Binding_Diagrammatic_Sketch](../images/ZK8_Client_Binding_Chart01.jpg)

#### On client-side

First, we have to get the client binder in order to use the client-side methods. To get the binder, simply use :

```javascript
var binder = zkbind.$('$id');
```
After we have our client binder, we can use the following two methods to interact with the view model back to our server.

**Method 1 - command**

```javascript
binder.command(commandName, data);
```
This method is used to trigger a command we have on our server.

**Parameters**
-   `commandName` - Command name on server-side (ViewModel).
-   `data`- JavaScript object, to pass any information you want with the command.

**Note : ** You could also pass ZK widgets in the data object and use @BindingParam to get the corresponding ZK component on the server.

**Additionally**, you could use `native component` or `client namespace` to publish a command.

```xml
<zk xmlns:n="native" xmlns:w="client">
	<button w:onClick=="@command('doClick', {key1:value1, key2:value2})"/>
	<n:button onClick="@command('doClick', {key1:value1, key2:value2})"/>
</zk>
```

**Method 2 - after**

```javascript
binder.after(commandName, callback);
```
This method is used to place a callback at the client after a command is executed on the server.

**Parameters**
-   `commandName` - Command name on server side (ViewModel).
-   `callback`- Callback function after the command gets executed on the server.

#### Server-Side

On server-side, we can use the following two annotations for the client-side binding. They should be placed at the beginning of the class declaration of our View Model.

**Annotation 1 - NotifyCommand**

```java
@NotifyCommand(value="commandName", onChange="_vm_.expression")
```

The notify command annotation allows us to trigger a command whenever the given expression changes at the server (by `@NotifyChange`). The `_vm_` here means the current view model.

Notice that the `commandName` which gets triggered is a command in our view model, and it would be triggered once in the initialization of `_vm_.expression`. If the command does not exist, it would do nothing. No matter whether the command exists in VM or not, it would pass to the following annotation - `@ToClientCommand`.

**Annotation 2 - ToClientCommand**

```java
@ToClientCommand(commandNames)
```

The client command annotation allows us to put the commands we want for notifying the client after execution. Notice only the commands we put inside this annotation will trigger the callback following *binder.after* at client.

###Examples
Two examples for using client-binding:
- ZK Blog :[
ZK8: Work with Polymer Components using ZK’s new client side binding API](http://blog.zkoss.org/index.php/2015/03/11/zk8-work-with-native-web-components-using-the-new-zk-client-side-data-binding-api/)
- ZK Small Talk :[ZK8 Series: Interact with Client Side Libaries using ZK's New Client Side Binding](http://books.zkoss.org/wiki/Small_Talks/2015/April/ZK8_Series:_Interact_with_Client_Side_Libaries_using_ZK8%27s_New_Client_Side_Binding)

