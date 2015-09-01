# @DependsOn

Syntax
======
``` java
@DependsOn
```

Description
===========
**Target:** getter method

**Purpose:** To notify change upon property's dependency.

It has the same function as ` @NotifyChange ` but inverse meaning. It's used to notify binder that getter method's target property is changed because one or more properties it depends on are changed. It can eliminate distributed ` @NotifyChange` annotations in a ViewModel when a calculated property depends on multiple properties.

Example
=======
``` java
public class FullnameViewModel {
    private String firstname;
    private String lastname;

    public String getFirstname() {
        return firstname;
    }
    public void setFirstname(String firstname) {
        this.firstname = firstname;
    }

    public String getLastname() {
        return lastname;
    }
    public void setLastname(String lastname) {
        this.lastname = lastname;
    }

    @DependsOn({"firstname", "lastname"})
    public String getFullname() {
        return (firstname == null ? "" : firstname) + " "
            + (lastname == null ? "" : lastname);
    }
}
```
