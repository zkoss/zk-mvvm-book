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


