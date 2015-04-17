# Collection and Selection

Property Binding in Collection Type
===================================
Assume that we have following ViewModel which has a collection type property:
```java
public class ItemsVM {

    private ItemService itemService = new ItemService();
    private List<Item> itemList = itemService.getAllItems();

    public List<Item> getItemList(){
        return itemList;
    }
}
```
For a ViewModel's ` Collection` type property, we usually bind it to those components that have **model** attribute like *Listbox*, *Grid*, or *Tree*. (You should not bind multiple attributes to a shared collection object,e.g. a static List, as concurrent issues may arise.)
```xml
    <grid width="400px" model="@bind(vm.itemList)">
```
If the property is one of Java Collection type object like ` List ` or ` Set `, an internal converter will convert the collection object as a [ListModel](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zul/ListModel.html) object automatically for user's convenience. But it has limitations that we will talk them in [later section](#choose-a-components-model-type).

Implicit Iteration Variable
---------------------------
After binding collection type property as a data source, we have to specify how to render each object of the model with [&lt;template&gt;](http://books.zkoss.org/wiki/ZK_Developer%27s_Reference/MVC/View/Template) and ZK will create components according to the fragment specified in ` <template>` iteratively. There are 2 implicit variables we can use in ` <template>`.

**each**, iteration object variable which references to each object of the model. We can use it to access an object's properties with dot notation, e.g. `each.name`.

**forEachStatus**, iteration status variable. it's used to get iteration index by `forEachStatus.index`.
```xml
<window apply="org.zkoss.bind.BindComposer"
viewModel="@id('vm') @init('org.zkoss.reference.developer.mvvm.collection.ItemsVM')">
    <grid width="400px" model="@bind(vm.itemList)">
        <columns>
            <column label="index" />
            <column label="name" />
        </columns>
        <template name="model" >
            <row>
                <label value="@bind(forEachStatus.index)" />
                <label value="@bind(each.name)" />
            </row>
        </template>
    </grid>
</window>
```
-   When using template element, we don't need to put `<rows>` inside a `<grid>`.

For better readability, we can also set template's "var" and "status" attribute to change implicit variable's name.

**var="myvar"**, set the iteration object variable's name. If you don't set "status" attribute at the same time, ZK will set iteration status variable's name to myvar**Status**, appending "Status" as the postfix.

**status="mystatus"**, set iteration status variable's name.

#### The ViewModel for tree example
```java
public class TreeVM {

    TreeModel<TreeNode<String>> itemTree;
    //omit getter and setter for brevity
}
```
```xml
<window apply="org.zkoss.bind.BindComposer"
    viewModel="@id('vm') @init('org.zkoss.reference.developer.mvvm.collection.TreeVM')">
    <tree model="@bind(vm.itemTree)" width="400px" >
        <treecols>
            <treecol label="name" />
            <treecol label="index" />
        </treecols>
        <template name="model" var="node" status="s">
            <treeitem>
                <treerow>
                    <treecell label="@bind(node.data)" />
                    <treecell label="@bind(s.index)" />
                </treerow>
            </treeitem>
        </template>
    </tree>
...
</window>
```
-   When using template element, we don't need to put `<treechildren>` inside a `<tree>`.

Collection Property Binding with Dynamic Template
-------------------------------------------------
Dynamic template allows developers to specify **which template to apply upon different conditions when rendering a container component**, e.g. *grid*. All components that support "model" attribute support dynamic template. You can use this feature in "model" attribute using the following syntax:

`@template( [EL-expression] )`

The binder will evaluate an EL expression as a template's name and look for corresponding template to render the child components. If the specified template doesn't exist in current component, it looks up the parent component's template. If no ` @template ` specified, it uses template that named **model** by default.

#### Template name specified
```xml
<grid model="@bind(vm.itemList) @template('myTemplate')">
    <template name="myTemplate">
    <!-- child components -->
    </template>
</grid>
```
-   Use the template whose name is specified in `@template`.

We could use EL to decide which template to use.

#### Conditional
```xml
<grid model="@bind(vm.itemList) @template(vm.type eq 'foo'?'template1':'template2')">
    <template name="template1">
    <!-- child components -->
    </template>

    <template name="template2">
    <!-- child components -->
    </template>
</grid>
```

We also can use implicit variable, **each** and **forEachStatus**, in EL expression of `@template()`. That means we can apply different template on different item in the binding collection dynamically.

#### Condition with implicit variables
```xml
<grid model="@bind(vm.itemList) @template(each.type eq 'A'?'templateA':'templateB')">
    <template name="templateA">
    <!-- child components -->
    </template>

    <template name="templateB">
    <!-- child components -->
    </template>

</grid>
```
-   Line 1: Assume that the object in binding collection has a property "type". Its value could be A or B.

Collection Property in Children Binding
---------------------------------------
For those components that do not support "model" attribute, we still can bind them to a collection type property with **children binding** ( please refer to [ Data Binding/Children Binding](./children_binding.html) to know basic concept.).

For those components that already support "model" attribute, we also can use children binding to change original component's rendering. For example, *Radio*s are arranged horizontally inside a *Radiogroup*, we can use children binding on *Vlayout* to arrange *Radios* vertically.

#### Children binding in radiogroup
```xml
<window apply="org.zkoss.bind.BindComposer"
    viewModel="@id('vm') @init('org.zkoss.reference.developer.mvvm.collection.ItemsVM')">
    <radiogroup>
        <vlayout children="@bind(vm.itemList)">
            <template name="children">
                <radio label="@load(each.name)" value="@load(each.name)"></radio>
            </template>
        </vlayout>
    </radiogroup>
</window>
```
### Combine with Dynamic Template
We can combine children binding with [dynamic template](./property_binding.html#dynamic-template)
in order to render different child components upon different conditions.

Here is an example to create a menu bar dynamically. If a menu item has no sub-menu, it creates *Menuitem* otherwise it creates *Menu*.

#### An example of dynamic menu bar
```xml
    <menubar id="mbar" children="@bind(vm.menuList) @template(empty each.children?'menuitem':'menu')">
        <template name="menu" var="menu">
            <menu label="@bind(menu.name)">
                <menupopup children="@bind(menu.children) @template(empty each.children?'menuitem':'menu')"/>
            </menu>
        </template>
        <template name="menuitem" var="item">
            <menuitem label="@bind(item.name)" onClick="@command('menuClicked',node=item)" />
        </template>
    </menubar>
```
The example above is to use a tree like data structure that the sub-template will render the content recursively.

#### Dynamic menu bar screenshot

![Mvvm-dynamic-menu](../images/Mvvm-dynamic-menu.png)

Selection in Collection
=======================
*Listbox* and *Tree* store user selection state including single or multiple selection. Single selection is enabled by default. The way to enable multiple selection depends on object's type you bind with "model"
attribute. ZK allows us to bind selection state with ViewModel's property.

Single Selection
----------------
For single selection state, ZK provides 2 kind of state. One is selected index and another is selected item of a model. We can create 2 properties in ViewModel for them respectively.

#### Properties for selection
```java
public class SingleSelectionVM{

    private ItemService itemService = new ItemService();
    private List<Item> itemList = itemService.getAllItems();
    private int pickedIndex;
    private Item pickedItem;

    //omit getter and setter for brevity
}
```
-   Line 5: pickedIndex is an integer for selected index.
-   Line 6: pickedItem's type should equal to the object's type in collection.

### Binding to Selected Index
To save or restore a component's selected index, we should bind **selectedIndex** attribute to a ViewModel's property with `@bind`. If we change the property's value for selection state in ViewModel, the component's selection state will also change.

#### Listbox selectedIndex example
```xml
<window apply="org.zkoss.bind.BindComposer"
viewModel="@id('vm') @init('org.zkoss.reference.developer.mvvm.collection.SingleSelectionVM')">
    <listbox  width="400px" model="@bind(vm.itemList)"
    selectedIndex="@bind(vm.pickedIndex)">
        <listhead>
            <listheader label="index"/>
            <listheader label="name"/>
        </listhead>
        <template name="model" var="item" status="s">
            <listitem>
                <listcell label="@bind(s.index)"/>
                <listcell label="@bind(item.name)"/>
            </listitem>
        </template>
    </listbox>
</window>
```

### Binding to Selected Item
To save or restore selected item we should bind **selectedItem** to a property whose type equals the object of the Model. In our example, we should bind "selectedItem" to an `Item` object.

#### Listbox selectedItem example
```xml
<window apply="org.zkoss.bind.BindComposer"
viewModel="@id('vm') @init('org.zkoss.reference.developer.mvvm.collection.SingleSelectionVM')">
    <listbox  width="400px" model="@bind(vm.itemList)"
    selectedItem="@bind(vm.pickedItem)" >
        <listhead>
            <listheader label="index"/>
            <listheader label="name"/>
        </listhead>
        <template name="model" var="item" status="s">
            <listitem>
                <listcell label="@bind(s.index)"/>
                <listcell label="@bind(item.name)"/>
            </listitem>
        </template>
    </listbox>
```
The usage is similar for *Tree*. Most components that support "model" attribute support above 2 properties, but some do not, e.g. *Tree* doesn't support **selectedIndex** attribute. (Please refer to each component's javadoc for supported attributes).

#### ViewModel for tree example
```java
public class TreeSelectionVM {

    private TreeModel<TreeNode<String>> itemTree;
    private String pickedItem;
    //omit getter and setter for brevity
}
```
#### Tree example
```xml
<window apply="org.zkoss.bind.BindComposer"
    viewModel="@id('vm') @init('org.zkoss.reference.developer.mvvm.collection.TreeSelectionVM')">
    <tree id="tree" model="@bind(vm.itemTree) " width="600px"
    selectedItem="@bind(vm.pickedItem)">
        <treecols>
            <treecol label="name" />
            <treecol label="index" />
        </treecols>
        <template name="model" var="node" status="s">
            <treeitem open="@bind(node.open)">
                <treerow>
                    <treecell label="@bind(node.data)" />
                    <treecell label="@bind(s.index)" />
                </treerow>
            </treeitem>
        </template>
    </tree>
</window>
```

#### Custom layout for Radiogroup
*Radiogroup* arranges its child *Radio* horizontally and users cannot change this arrangement but we could use children binding to arrange *Radio* vertically. In order to make "selectedItem" work correctly, we should put `<radio>` before `<radiogroup>`.

> Since 6.5.1

We can bind "selectedItem" with any type of object, not only String.
```xml
         <vlayout children="@load(vm.itemList)">
            <template name="children">
                <radio label="@load(each.name)" value="@load(each)" radiogroup="rg" />
            </template>
        </vlayout>
        <radiogroup id="rg" selectedItem="@bind(vm.pickedItem)"/>
```

### Binding to Selected Item with Static UI Data
When using static data, we still can get and save "selectedItem" with similar rule. In the following example, we should bind "selectedItem" with a String property. Because we set String to *Radio*'s value. Their types have to match.

#### Static data model
```xml
        <radiogroup selectedItem="@bind(vm.pickedItemName)">
            <radio label="Item 0" value="Item 0" />
            <radio label="Item 1" value="Item 1" />
            <radio label="Item 2" value="Item 2" />
        </radiogroup>
```
-   Line 1: `vm.pickedItemName` is a String property.

In previous section, we have said that type of object "selectedItem" attribute bound should equal the object of the Model. When using static data ( no use model), the select component's value type should equal to the type of property for "selectedItem". Because ZK restores the selection state by comparing "selectedItem"  attribute and selected component's "value" attribute.

### Custom Single Selection
Because *Tabbox* doesn't support "model" attribute, we should do a little extra work to keep selection state. We can create a ViewModel with a variable for selected item and create a command method to save selected item passed by command binding.
```java
public class CustomSingleSelectionVM {

    private ItemService itemService = new ItemService();
    private List<Item> itemList = itemService.getAllItems();
    private Item pickedItem = itemList.get(0);

    //omit setter and getter for brevity
    @NotifyChange("pickedItem")
    @Command
    public void select(@BindingParam("item") Item item){
        pickedItem = item;
    }
}
```
First we use children binding to create *Tab* and *Tabpanel* dynamically then write a command binding on "onSelect" to pass selected item. Then we can set "selected" attribute by comparing selected item with the
object bound with each *Tab*.
```xml
<div apply="org.zkoss.bind.BindComposer" width="600px"
    viewModel="@id('vm') @init('org.zkoss.reference.developer.mvvm.collection.CustomSingleSelectionVM')">
    <tabbox>
        <tabs children="@load(vm.itemList)">
            <template name="children">
                <tab label="@load(each.name)" selected="@load(vm.pickedItem eq each)"
                    onSelect="@command('select',item=each)" />
            </template>
        </tabs>
        <tabpanels children="@load(vm.itemList)">
            <template name="children">
                <tabpanel height="200px">
                    <label value="@load(each.name)" />
                </tabpanel>
            </template>
        </tabpanels>
    </tabbox>
</div>
```

Multiple Selections
-------------------
Some components support multiple selections, but it needs to be enabled before using it. The way to enable multiple selections depends on property's type you bind with "model" attribute. If the property type is Java Collection type such as `List, Set, or Map `, we should specify `multiple="true"` on the component to enable multiple selections. If a property's type is `ListModel` and it implements [Selectable](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zul/ext/Selectable.html), we should call `setMultiple(true)` to enable it.

To keep multiple selections state, we should bind **selectedItems** to a property whose type is `Set`.

#### Selected Set
```java
public class MultipleSelectionsVM{

    private ItemService itemService = new ItemService();
    private List<Item> itemList = itemService.getAllItems();
    private Set pickedItemSet;

    //omit getter and setter for brevity
}
```
#### Multiple selections enabled Listbox
```xml
<window apply="org.zkoss.bind.BindComposer"
viewModel="@id('vm') @init('org.zkoss.reference.developer.mvvm.collection.MultipleSelectionsVM')">
    <listbox  width="400px" model="@bind(vm.itemList)" checkmark="true" multiple="true"
    selectedItems="@bind(vm.pickedItemSet)" >
        <listhead>
            <listheader label="index"/>
            <listheader label="name"/>
        </listhead>
        <template name="model" var="item" status="s">
            <listitem>
                <listcell label="@bind(s.index)"/>
                <listcell label="@bind(item.name)"/>
            </listitem>
        </template>
    </listbox>
</window>
```
-   Line 3: Because `vm.itemList` is Java list object, we can enable multiple selection by set "multiple" to "true".

### Custom Multiple Selections
Some components don't support "selectedItems" attribute like *Checkbox*, we still can keep selection state by passing arguments. Create the following ViewModel contains a set to store selected items and a command that update items in the set according to passed arguments.
```java
public class CustomMultipleSelectionsVM {

    private ItemService itemService = new ItemService();
    private Set<Item> pickedItemSet = new HashSet<Item>();

    @Command
    @NotifyChange("pickedItemSet")
    public void pick(@BindingParam("checked") boolean isPicked, @BindingParam("picked")Item item){
        if (isPicked){
            pickedItemSet.add(item);
        }else{
            pickedItemSet.remove(item);
        }
    }

    //omit getter and setter for brevity
}
```
-   Line 8: For details of argument annotations, please refer to [Advanced/Parameters](../advanced/parameters.html).

*Checkbox* doesn't support "model" attribute, so we use children binding to render them upon item list. We also bind "onCheck" attribute to a command and pass selected `Item` object to command method for updating selected set.

#### Selected items for checkboxes
```xml
<window apply="org.zkoss.bind.BindComposer"
    viewModel="@id('vm') @init('org.zkoss.reference.developer.mvvm.collection.CustomMultipleSelectionsVM ')">
    <vlayout id="vlayout" children="@load(vm.itemList)">
        <template name="children">
            <checkbox label="@load(each.name)"
                onCheck="@command('pick', checked=self.checked, picked=each)">
            </checkbox>
        </template>
    </vlayout>
    <label value="@bind(vm.pickedItemSet)"/>
</window>
```
-   Line 6: Pass arguments in command binding to update selected items.

Choose a Component's Model Type
===============================
As we mentioned in previous section, we could bind a Java Collection type property to be a component's model . An internal converter will convert it to [LIstModel](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zul/LIstModel.html) automatically
for us. It is convenient but one of its cost is "scroll position lost when collection size changes". If you scroll to an item of index 50 and delete (or add) an item. The component will force to scroll back to first item because it has to re-convert and re-render all items. One simple way to handle this is to set *Listbox* as "paging" mold. Another way is to wrap the collection object with [ListModelList](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zul/ListModelList.html).

One more issue about performance is *Listbox* re-renders all items <sub>[1]</sub> when using Java collection object as the model in default mold. When you change the collection property by removing or adding an item, you have to notify its change. This will trigger *Listbox* to re-render from the beginning. But if you use `ListModelList` as a model and modify it by calling its method `add()` and `remove()`. `ListModelList` will update to client automatically without explicitly specifying property notification. And this update only re-renders the affected (added or removed) item instead of all items which is more efficient.<sub>[2]</sub>

But if you have huge amount of data, and getting them takes unbearable long time. You should implement your own model object which we will talk in [Advance](../advanced/displaying_huge_amount_of_data.html) section.

[1]: If ROD enabled, it only renders visible items .

[2]: If you still specify property notification for `ListModelList` property, *Listbox* renders all items (If ROD is disabled). Then you won't gain performance improvement.

#### Use Java List as a model
```java
public class ModelTypeVM extends SingleSelectionVM{
    protected List<Item> itemList;

    @Init
    public void init(){
        pickedItem = new Item();
        itemService = new ItemService(100);
        itemList = itemService.getAllItems();  //return a java.util.List
        ...
    }

    @Command @NotifyChange({"itemList","pickedItem"})
    public void add(){
        itemList.add(pickedItem);
        pickedItem = new Item();
    }

    @Command  @NotifyChange({"itemList","pickedItem"})
    public void delete(){
        int index = itemList.indexOf(pickedItem);
        if (index != -1){
            itemList.remove(index);
            pickedItem = new Item();
        }

    }

    //omit setter and getter
}
```
-   Line 8: `itemList` is java.util.List object.
-   Line 12, 18: Because we change the property "itemList", we should notify binder to reload it.

#### Use ListModelList as a model
```java
public class ModelTypeVM extends SingleSelectionVM{

    private ListModelList<Item> itemListModel;

    @Init
    public void init(){
        pickedItem = new Item();
        itemService = new ItemService(100);
        ...
        itemListModel = new ListModelList<Item>(itemService.getAllItems());
    }

    @Command @NotifyChange("pickedItem")
    public void modelAdd(){
        itemListModel.add(pickedItem);
        pickedItem = new Item();
    }

    @Command  @NotifyChange("pickedItem")
    public void modelDelete(){
        int index = itemListModel.indexOf(pickedItem);
        if (index != -1){
            itemListModel.remove(index);
            pickedItem = new Item();
        }

    }

    //omit setter and getter
}
```
-   Line 10: We can simply wrap a java.util.List by passing it into `ListModelList`'s constructor.
-   Line 13, 20: Because `ListModelList` will update change to clients automatically, we can ignore to notify the property "itemListModel".
