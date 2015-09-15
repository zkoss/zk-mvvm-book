# @template

Syntax
======

` @template(`*`[EL-expression]`*`) `

Description
===========

**Target Attribute:** model, children

**Purpose:** It should be used with ` @bind `, ` @load `. It determines which template should be used to render child components.

The evaluation result of EL expression should be the name of a template which is used to render child components. The result template's name can be any template defined in the same id space.

Example
=======

#### Dynamic template upon iteration status variable
``` xml
<combobox model="@bind(item.options ) @template(forEachStatus.index eq 0 or forEachStatus.index eq 2 ? 'model1' : 'model2')">
    <template name="model1" var="option">
        <comboitem label="@bind(optionStatus)" description="@bind(option)"/>
    </template>
    <template name="model2" var="option">
        <comboitem label="@bind(optionStatus)" description="@bind(option)"/>
    </template>
</combobox>
```

#### Recursive usage
``` xml
<vlayout id="vlayout" children="@load(vm.nodes) @template('greenBox')">
    <template name="greenBox" var="node">
        <vlayout style="padding-left:10px; border:2px solid green;">
            <label value="@bind(node.name)"/>
            <vlayout children="@load(node.children) @template('greenBox')"/>
        </vlayout>
    </template>
</vlayout>
```

-   vm.nodes has a tree-like structure, and node.children is a collection of node.

