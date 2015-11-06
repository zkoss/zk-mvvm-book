# @NotifyCommand
> Since 8.0.0

Syntax
======

``` java
@NotifyCommand(value="commandName", onChange="_vm_.expression")
```

Description
===========

**Target:** class

**Purpose:** The notify command annotation allows us to trigger a command whenever the given expression changes at the server (by `@NotifyChange`). The `_vm_` here means the current view model.

Notice that the `commandName` which gets triggered is a command in our view model, and it would be triggered once in the initialization of `_vm_.expression`. If the command does not exist, it would do nothing and pass to another annotation - `@ToClientCommand`.

Example
=======

``` java
@NotifyCommand(value="upateData", onChange="_vm_.data")
public class VM {
    private Map<String, String> data = new HashMap<String, String>();

    @Command
    @NotifyChange("data")
    public void upateData() {
        data.put("Status", "updated");
    }
}
```

#### Combine with `@ToClientCommand` :

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
