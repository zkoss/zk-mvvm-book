# @ToServerCommand
> Since 8.0.0

Syntax
======

``` java
@ToServerCommand(commandNames)
```

Description
===========

**Target:** class

**Purpose:** Put the commands we want to notify the server triggered by the client binder. Notice only the commands we put inside this annotation will be received from server.

**Note:** if the value of the annotation contains a value __"*"__ that means all of the commands are accepted to notify the server.
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


