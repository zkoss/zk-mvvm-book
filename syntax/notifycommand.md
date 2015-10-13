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

**Purpose:** Trigger a command in viewmodel whenever the given expression changes on the server  (by @NotifyChange). The `_vm_` here means the current view model.

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




