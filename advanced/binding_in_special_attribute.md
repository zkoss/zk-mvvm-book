# Binding in Special Attribute

Special Attribute Issue
=======================
ZK Bind is a post-processing work on components after they have been created and it can control most attributes to change a component's status. However, there are [some special attributes](http://books.zkoss.org/wiki/ZUML_Reference/ZUML/Attributes) such as `if` and
`forEach` in which ZK Bind can't work on because these attributes' value are determined and fixed when components are created. Therefore, binding these special attributes takes no effect on components, but you may want to use the functions that these special attributes provide, here we demonstrate alternative methods in MVVM approach.

The "if" Versus the "visible"
=============================
Assume that you want to show a "Delete" button only to a user who has administrative permission. There are several usages:

#### specialAttribte.zul
```xml
<!-- wrong usage, no effect -->
<button label="Delete " if="@load(vm.currentUser.admin)"/>
<!-- determined at the beginning -->
<button label="Delete (EL)" if="${vm.currentUser.admin}"/>
<!-- can change during user interaction -->
<button label="Delete (visible)" visible="@load(vm.currentUser.admin)"/>
<button label="Delete (disabled)" disabled="@load(not vm.currentUser.admin)"/>
<checkbox label="Is Admin" checked="@bind(vm.currentUser.admin)"/>
```
-   Line 2: It is a wrong usage; the delete button will never be created.
-   Line 4: The button's creation is determined when a user visits the page and won't appear unless the user becomes a administrator andivsits the page again.
-   Line 6,7: It is what we recommend for most cases. You can bind on `visible`, and it brings you almost the same effect as `if`. The `disabled` also has similar effect.

The "forEach" Versus Children Binding
=====================================
The `forEach` attribute also has the same issue.

#### specialAttribute.zul
```xml
<!-- "forEach" versus children binding  -->
<!-- wrong usage, no effect -->
<checkbox label="@load(each.firstName)" forEach="@load(vm.personList)"/>
<!-- determined at the beginning -->
<checkbox label="${each.firstName}" forEach="${vm.personList}"/>
<!-- children binding -->
<div children="@load(vm.personList)">
    <template name="children">
        <checkbox label="@load(each.firstName)"/>
    </template>
</div>
```
-   Line 3: It is a wrong usage; it doesn't create multiple checkboxes.
-   Line 5: The checkboxes are created at the beginning when a user visits the page and won't change even if we change `vm.personLIst`.
-   Line 7: [ Children binding](../data_binding/children_binding.html) is used when you want to dynamically create and destroy components.

Binding values to client attribute
=====================================
ZK component may receive client attributes using the xml namespace "[client/attribute](https://www.zkoss.org/wiki/ZUML_Reference/ZUML/Namespaces/Client_Attribute)".
Client-attributes are added to the rendering information of components, and must be follow the same rules as the special attributes.

The client attribute must be initialized using the `${value}` syntax, and doesn't support `@init`, `@load` or `@bind`.

#### specialAttribute.zul
```xml
<!-- "forEach" versus children binding  -->
<div xmlns:ca="client/attribute">
    <!-- correct use for non-dynamic attribute values -->
    <checkbox ca:attribute="${vm.value}" />
    <!-- binding a referenced value or dynamic value by recreating content after triggering a binding using shadow element apply -->
    <apply refvalue="@load(vm.value)">
        <!-- apply content is recreated when the binding expression "vm.value" is notified -->
        <!-- using ${value} is correct, since expression is re-evaluated once the apply content is recreated -->
        <checkbox ca:attribute="${refvalue}" />
    </apply>
</div>
```
