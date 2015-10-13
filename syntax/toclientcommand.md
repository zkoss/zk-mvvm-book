# @ToClientCommand
> Since 8.0.0

Syntax
======

``` java
@ToClientCommand(commandNames)
```


Description
===========

**Target:** class

**Purpose:** Specify the commands that will be used for notifying the client after execution. Notice only the commands placed inside this annotation will trigger the callback following *binder.after* at client.

**Note:** if the value of the annotation contains a value __"*"__ this means all of the commands are accepted for notifying the client.
Example
=======

``` java
@ToClientCommand("doCountChange")
public class VM{
    private int count = 0;

    // getter/setter...

    @Command
    public void doCountChange(){
       count++;
    }
}
```

```xml
<window viewModel="@id('vm') @init('org.zkoss.VM')" xmlns:n="native">
       <n:div id="display"></n:div>
       <n:script>
       	zk.afterMount(function() {
			var binder = zkbind.$('$display');
			// the event handler of after 'doCountChange' from server
			binder.after('doCountChange', function() {
				alert("after doCountChange");
			});
		});
       </n:script>
       <button label="change" onClick="@command('doCountChange')" />
</window>
```
Combine with `@NotifyCommand` :

``` java
@NotifyCommand(value="upateData", onChange="_vm_.count")
@ToClientCommand("upateData")
public class VM{
    private int count = 0;

    // getter/setter...

    @Command
    @NotifyChange("count")
    public void doCountChange(){
       count++;
    }
}
```
Once trigger the command "doCountChange", VM would notify the change of "count".
Then server would trigger the callback following *binder.after* at client.

Notice that the command "upateData" in VM is not necessary. If the command "updateData" in VM exist, it would be executed first, then server would trigger the callback function.

