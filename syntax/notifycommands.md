# @NotifyCommands
> Since 8.0.0

Syntax
======

``` java
@NotifyCommands({
	@NotifyCommand(value="commandName1", onChange="_vm_.expression1"),
	@NotifyCommand(value="commandName2", onChange="_vm_.expression2")
})
```

Description
===========

**Target:** class

**Purpose:** Trigger an array of commands in viewmodelwhenever the given expression changes at the server (by `@NotifyChange`). The `_vm_` here means the current view model.

Notice that the `commandName` which gets triggered is a command in our view model, and it would be triggered once in the initialization of `_vm_.expression`. If the command does not exist, it would do nothing and pass to another annotation - `@ToClientCommand`.

Example
=======

``` java
@NotifyCommands({
	@NotifyCommand(value="upateData1", onChange="_vm_.data1"),
	@NotifyCommand(value="upateData2", onChange="_vm_.data2")
})
public class VM {
    private Map<String, String> data1 = new HashMap<String, String>();
    private Map<String, String> data2 = new HashMap<String, String>();

    @Command
    @NotifyChange("data1")
    public void upateData1() {
        data1.put("Status", "updated");
    }

    @Command
    @NotifyChange("data2")
    public void upateData2() {
        data2.put("Status", "updated");
    }
}
```
#### Combine with `@ToClientCommand` :

``` java
@NotifyCommands({
	@NotifyCommand(value="upateData1", onChange="_vm_.data1"),
	@NotifyCommand(value="upateData2", onChange="_vm_.data2")
})
@ToClientCommand({"upateData1", "upateData2")
public class VM{
    private int count = 0;

    // getter/setter...

    @Command
    @NotifyChange({"data1", "data2"})
    public void doCountChange(){
       count++;
    }
}
```
Once trigger the command "doCountChange", VM would notify the change of "count".
Then server would trigger the callback following *binder.after* at client.

Notice that the command "upateData1" and "upateData2" in VM is not necessary. If the command "updateData1" in VM exist, it would be executed first, then server would trigger the callback function.
