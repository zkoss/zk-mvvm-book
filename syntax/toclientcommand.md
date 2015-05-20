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

**Purpose:** Put which commands we want to notify the client after execution has been done. Notice only the commands we put inside this annotation will trigger the callback we put in *binder.after* at client.

**Note:** if the value of the annotation contains with a value __"*"__ that means all of the commands are accepted to notify the client.
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


