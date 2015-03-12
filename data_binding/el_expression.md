# EL Expression

EL Expression in Data Binding
=============================
In ZK bind annotation, we adopt EL expression to specify a binding target and reference an implicit object. The binding target is mostly a ViewModel's (nested) properties. You can use EL expression in a ZUL
which is described in the section [ZK Developer's Reference/UIComposing/ZUML/EL Expressions](). But using EL in ZK bind annotation is a little bit different in format and evaluation.
#link not yet!

Basic Format
------------
[ All ZK bind annotation]() has the general format:
```xml
@[Annotation](value=[EL-expression], [arbitraryKey]=[EL-expression])
```
It starts from a "@" symbol followed by an annotation's name like “id” or “bind”. Inside parentheses we can write multiple **key-value pairs** separated with a comma. The key is a self-defined name (not an EL
expression), and it's like a key in a Map. The value is an EL expression but is **not enclosed with "${" and "}"**. The default key name is **“value”**. If you only write a EL expression without specifying its key name, it's implicit set to key named “value”. Hence we usually omit this default key name when writing ZK bind annotation. In most case, we can just write a annotation as follows:
```xml
@[Annotation]( [EL-expression])
```
We just need to write one key-value pair and omit default key name. We often use multiple key-value pairs for passing parameters to command, converter, and validator.

Although all ZK bind annotation has the general format, the way how a binder parse and evaluates the EL expression is different among different annotations. We'll describe the differences in the following sections.

Run-time Evaluation
-------------------
A binder usually evaluates an EL expression each time when it wants to access the target object. Hence The evaluation result might be different from time to time.

### Command binding according to run-time value
```xml
<button label="Cmd" onClick="@command(vm.checked?'command1':'command2')" />

<groupbox visible="@load(not empty vm.selected)" />
```
* When clicking the button, the binder executes a command upon value of “vm.checked”.

### Indirect reference
```xml
<label value="@bind(vm.person[vm.currentField])"/>
```
- If evaluation result of `vm.currentField` is firstName, the binder loads `vm.person.firstName`. So which property of `vm.person` that a binder loads depends on `vm.currentField`'s evaluation result.

Call ViewModel Methods
----------------------
You can use EL expression to call a method in a ViewModel.
### Call concat() of a ViewModel
```xml
<label value="@load(vm.concat(vm.foo,'postfix'))"/>
```

Call Tag Library's Methods
--------------------------
We could call [tag library's methods in a zul with EL expression](http://books.zkoss.org/wiki/ZUML_Reference/ZUML/Processing_Instructions/taglib) equally, we can also call them in a data binding expression and this calling can be nested. The syntax is as follows:
```xml
<?taglib uri="http://www.zkoss.org/dsp/web/core" prefix="c"?>
...
    <label value="@load(c:cat('A-',vm.value))" />
    <label value="@load(c:cat3('$',c:formatNumber(vm.price, '00'), ' per person'))" />
```
* Line 1: Declare the tag library for core methods.
* Line 3: core's cat method.
* Line 4: Nested calling with core's cat3 method and formatNumber.

Call Xel Methods
----------------------
The [xel method](http://books.zkoss.org/wiki/ZUML_Reference/ZUML/Processing_Instructions/xel-method) allows developers to call a Java class's static method in EL expression without defining a tag library. You can also use this feature in data binding expression
```xml
<?xel-method prefix="x" name="max"
    class="java.lang.Math"
    signature="int max(int,int)"?>
...
    <intbox value="@bind(vm.value1)"/>
    <intbox value="@bind(vm.value2)"/>
    <label value="@load(x:max(vm.value1,vm.value2))"/>
```

Tips
----------------
Some EL characters are illegal in XML attribute or ZK annotation, you should replace them with other EL operators.

| Character | Replacement |
| -- | -- |
| = | eq |
| != | ne |
| && | and |
| < | lt |
| <= | le |
| > | gt |
| >= |  e|
For example:
```xml
<image src="@load(vm.picture ne null ? 'images/'.concat(vm.picture) : 'images/NoImage.png')"/>

<label value="@bind(vm.age lt 18?'true':'false')"/>
```
