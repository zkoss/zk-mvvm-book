# Using NavigationModel
> Since 8.6.0

By the help of **[NavigationModel](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zuti/zul/NavigationModel.html)** and `<apply>`, we can construct a multi-level navigation and load template of each level to be applied with ease.

Construct a Model
=================

#### Create a navigation object
```java
public class MyObject {
    private String name;
    private String description;

    public MyObject(String name, String description) {
        this.name = name;
        this.description = description;
    }
    // getters and setters are omitted
}
```

#### Create a NavigationModel in the ViewModel
```java
public class MyVM {
    private NavigationModel<MyObject> navModel = new NavigationModel<MyObject>();
 
    public MyVM() {
        navModel.put("AAA", new MyObject("AAA", "desc 1"));
        navModel.put("AAA/AAA1", new MyObject("AAA1", "desc 2"));
        navModel.put("BBB", new MyObject("BBB", "desc 3"));
        navModel.put("BBB/BBB1", new MyObject("BBB1", "desc 4"));
        navModel.put("CCC", new MyObject("CCC", "desc 5"));
    }
 
    public NavigationModel<MyObject> getNavModel() {
        return navModel;
    }
}
```
- We declared a `NavigationModel` for storing many `MyObject`.
- The path string `AAA/AAA1` means level 1 is AAA and level 2 is AAA1.

#### Nested level in ZUML
``` xml
<apply level="@load(vm.navModel.root)">
    // do some level 1 thing...
    <apply level2="@load(level.child)">
        // do some level 2 thing...
    </apply>
</apply>
```

Render a level
==============

In each level, we defined different properties like `level` or `level2` to store the **[NavigationLevel](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zuti/zul/NavigationLevel.html)** object. So we can get the current navigated object by calling `getCurrent()`.

#### ZUML
``` xml
<apply level="@load(vm.navModel.root)">
    Name: <label value="@load(level.current.name)" />
</apply>
```

Navigate a level
================

You can bind a `@command` which is calling `navigateTo` for navigating items.

#### ZUML
``` xml
<a label="Navigate to BBB"
   onClick="@command('navTo', level=level, key='BBB')" />
```

#### ViewModel
``` java
@Command
public void navTo(@BindingParam("level") NavigationLevel level,
                  @BindingParam("key") String key) {
    level.navigateTo(key);
}
```

`NavigationModel` also provides the method for navigating many levels at once, `navigateToByPath`.

``` java
@Command
public void navByPath(@BindingParam("path") String path) {
    this.navModel.navigateToByPath(path);
}
```

Iterate a level
===============

To list all items in a level, use `getItems()` or `getItemIterator()` to a list of `Pair` objects which x is key and y is value.

#### ZUML
``` xml
<forEach items="@load(level.items)">
    <a label="@init(each.x)"
       onClick="@command('navTo', level=level, key=self.label)" />
</forEach>
```