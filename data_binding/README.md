# Data Binding

# Overview
The data binding mechanism plays key role in synchronizing data between the View and ViewModel when developing a web application using the MVVM pattern.

Data binding is a mechanism to ensure that any change made to the data in a UI components is automatically carried over to the target ViewModel based on a predefined binding relationship. (and vice versa). Application developers only have to define the data binding relationship between UI component's attribute and the target object, it's usually a ViewModel, by data binding annotation expression.

**ZK Bind** is a whole new data binding mechanism with new specifications and implementations. Based on the experiences learned from data binding 1 and taking into account suggestions and feedback from users and contributors, we have created this easy to use, flexible, feature-rich new data binding system in ZK6.

![MVVM DataBinding Role](http://books.zkoss.org/images/7/7e/Mvvm-databinding-role.png)

Developers use ZK bind annotation to define various data binding relationship. The **binding source** is a component's attribute and **binding target** is a ViewModel's property or Command. When we bind a component to a ViewModel, that component becomes the ViewModel's **Root View Component**. Attributes of all child components and root view component itself can be bound to the ViewModel's properties (or command) through ZK bind annotation. This Root View Component doesn't have to be the root component of a page,

For example:
```xml
<vlayout>
    <vbox apply="org.zkoss.bind.BindComposer" viewModel="@id('vm') @init('foo.MainViewModel')" >
        <label value="@load(vm.msg)" />
    </vbox>
</vlayout>
```
* The vbox is the Root View Component of ViewModel "foo.MainViewModel"
* The label's value attribute is binding source, ViewModel's msg property is binding target.

# Quick Preview

## Annotation Expression
* Java style annotation expressions in zul: The new ZK annotation is consistent with Java's annotation style. If you know the Java style, you know the ZK Style.
* A set of collaborated annotations: ZK Bind uses a set of annotations to make the use of data binding as intuitive and clear as possible.
    * **@id(...)**: used to identify a instance's id , ex a view model or a form.
    * **@init(...)**: used to initialize a instance
    * **@load(...)**: used to bind data and command along with parameters for loading data to target
    * **@save(...)**: used to bind data and command along with parameters for saving data.
    * **@bind(...)**: used to bind data along with parameters for both loading and saving.
    * **@ref(...)**: used to make a reference for an expression (since 6.0.1).
    * **@converter(...)**: used to specify converter along with parameters
    * **@validator(...)**: used to specify validator along with parameters
    * **@command(...)**: used to bind an event to a command along with parameters.
    * **@global-command(...)**: used to bind an event to a global command along with parameters.
    * **@template(...)**: used to determine the template to render a container component.

## EL 2.2 Flexible Expressions
* ZK Bind accepts EL 2.2 syntax expressions in which you can provide flexible operations easily.
* Bind to bean properties, indexed properties, Map keys seamlessly.
* Bind to component custom attributes automatically.
* Bind to Spring, CDI, and Seam managed bean automatically.
```xml
<image src="@load(vm.person.boy ? 'boy.png' : 'girl.png')"/>
<button onClick="@command(vm.add ? 'add' : 'update')" label="@load(vm.add ? 'Add' : 'Update'"/>
<button onClick="@command('subscribe')" disabled="@load(empty vm.symbol)" label="Subscribe"/>
```

## One Way Load Only
* Load when bean property changes
* Conditional load after/before executing a command
* Multiple conditional load before/after executing different/same commands
```xml
<label value="@load(vm.person.fullname)"/>
<label value="@load(vm.person.firstname, after='update')"/>
<label value="@load(vm.person.firstname, before='delete')"/>
<label value="@load(vm.person.firstname, after={'update','delete'})"/>
<label value="@load(vm.person.firstname, after='update') @load(vm.person.message, after='delete')"/>
```

## One Way Save Only
* Save when UI component attribute changes
* Multiple save to property of different target beans
* Conditional save before/after executing a command
* Multiple conditional save before/after executing different/same commands
```xml
<textbox value="@save(vm.person.firstname)"/>
<textbox value="@save(vm.person.firstname) @save(vm.tmpperson.firstname)"/>
<textbox value="@save(vm.person.firstname, before='update')"/>
<textbox value="@save(vm.person.firstname, after='delete')"/>
<textbox value="@save(vm.selected.firstname, before={'update','add'})"/>
```

## Initial Binding
* Loads when UI components are first added into the binding system
```xml
<label value="@init(vm.selected.firstname)"/>
```

##Two Way Data Binding
* Multiple conditional load and save on different back-end beans before/after executing different/same commands
```xml
<textbox value="@load(vm.selected.firstname) @save(vm.selected.firstname) @save(vm.newperson.firstname,before='add')"/>
```
* Short expression for both save and load bindings without command condition.<sub>[1]</sub>
```xml
<textbox value="@bind(vm.person.firstname)"/>
```
[1]: `@bind(...)` is shortcut for "`@load(...) @save(...)`", and `@save` is automatically ignored if the property doesn't support it

## Bind to Any Attributes
* Bind to all attributes of UI components
```xml
<textbox value="@bind(vm.symbol)" instant="true"/>
<button disabled="@load(empty vm.symbol)" label="Subscribe" />
```

## Event Command Binding
* Bridge ZK event to command
* Automatic event listener registration
* Simple command name invocation
```xml
<button onClick="@command('subscribe')" disabled="@load(empty vm.symbol)" label="Subscribe" />
```

## Collection Binding
* Binding on Listbox/Grid/Tree/Combobox
* Local variable scope is limited to the container component
* Support index property (`fooStatus.index` in follow example)
```xml
<listbox width="300px" model="@load(vm.albumList)" selectedItem="@bind(vm.selectedAlbum)" vflex="true">
    <template name="model" var="foo">
        <listitem label="@load(foo.title)"/>
        <listitem label="${fooStatus.index}" />
    </template>
</listbox>
```

## Reference Binding
> Since 6.0.1

* Make a reference for an EL expression with a custom name
* Make an expression shorter
* Modularize the view to make it reusable
```xml
<vlayout p="@ref(vm.person)">
    <hlayout>
        First Name: <textbox value="@bind(p.firstName)" />
    </hlayout>
    <hlayout>
        Last Name: <textbox value="@bind(p.lastName)" />
    </hlayout>
</vlayout>
```

## Embedded Validation Cycle
* Bind validator by name or by EL expression
* Embedded system Validator: provide commonly used validators in which users can use directly by only specifying the name
* Validate a single property or a form
* Validate upon a command
```xml
<textbox value="@save(vm.selected.firstname) @validator('beanValidator')"/>
<grid form="@id('fx') @load(vm.selected) @save(vm.selected, before='update') @validator(vm.passwordValidator)">
    <row>username<textbox value="@bind(fx.username)"/></row>
    <row>password<textbox value="@bind(fx.password)" type="password"/></row>
    <row>retype password<textbox value="@bind(fx.retypePassword)" type="password"/></row>
</grid>
```

## Enhanced Converter Mechanism
* Bind converter by name or by EL expression
* Embedded system Converters: provide commonly used converters in which users can use directly by only specifying the name
```xml
<datebox value="@bind(vm.selected.birthday) @converter('formattedDate', format='yyyy/MM/dd')"/>
```

## Dynamic Template
* Dynamically determine which template to render.
```xml
<grid model="@bind(vm.nodes) @template(vm.type='foo'?'template1':'template2')">
    <template name="template1">
    <!-- child components -->
    </template>

    <template name="template2">
    <!-- child components -->
    </template>
</grid>
```

## Debugging
ZK supports debug features such as printing ZK bind debugging information that can be enabled manually. Please refer to [MVVM/Configuration]().
#link no yet!!
