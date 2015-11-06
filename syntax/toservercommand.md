# @ToServerCommand
> Since 8.0.0

Syntax
======

``` java
@ToServerCommand("commandName")
@ToServerCommand({"commandName1", "commandName2"})
```

Description
===========

**Target:** class

**Purpose:** Specify the client binder commands that will be used to notify the server. Only the commands placed inside this annotation will be sent to the server.

**Note:** If the value of the annotation contains a value __"*"__ that means all of the commands are accepted for notifying the server.
Example
=======

``` java
@ToServerCommand("doCountChange")
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
			// this will trigger the server side command 'doCountChange' to execute once.
			binder.command('doCountChange');
		});
       </n:script>
</window>
```


