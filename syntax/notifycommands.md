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

**Purpose:** Trigger an array of commands in viewmodel whenever the given expressions change on the server (by @NotifyChange). The `_vm_` here means the current view model.

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
