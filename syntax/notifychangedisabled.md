# @NotifyChangeDisabled

Syntax
======
``` java
@NotifyChangeDisabled
```

Description
===========
**Target:** setter method

**Purpose:** Marker annotation to disable default notification behavior.

To disable the default notification when binder sets a property.

Example
=======
``` java
public class OrderVM {

    //other code...

    @NotifyChangeDisabled
    public void setSelected(Order selected) {
        this.selected = selected;
    }
}
```
