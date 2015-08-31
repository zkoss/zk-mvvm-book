# @Command

Syntax
======
``` java
@Command()

@Command("commandName")

@Command({"commandName1", "commandName2"})
```

Description
===========
**Target:** method

**Purpose:** To identify a Command method.

The optional annotation's element is a String for command's name and that name is referenced in a ZUL with event-command binding. If it's not provided, the method name is set as the command name by default.

We also can use parameter related annotation on initial method's parameters, please refer to subsections of [ Parameters](./parameters.html) for more information.

Example
=======
#### Method name as command name
``` java
@Command
public void search() {
    items = new ListModelList<Item>();
    items.addAll(getSearchService().search(filter));
    selected = null;
}
```
#### Specify command name
``` java
@Command("delete")
public void deleteOrder() {
    getService().delete(selected); //delete selected
    getOrders().remove(selected);
    selected = null; //clean the selected
}
```
