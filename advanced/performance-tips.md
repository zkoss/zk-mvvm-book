# Performance Tips

## Choose data bindings carefully

`@init`, `@load`, `@save`, and `@bind` are most common data bindings.  The `@init` will load a ViewModel's value just once and don't track the value anymore which is different from other 3 annotations. For tracking changes of bound properties, ZK will create tracking nodes in memory for those annotations except `@init`, and those tracking nodes consume a server's memory.

By expensiveness in memory, they are ranked as follows: `@init` < `@load` or `@save` < `@bind`.

Hence, to save a server's memory, use `@init` if you don't need to reload a value when it's changed. For example, A binding to an object of a collection doesn't need to be reloaded for displaying data only, so it's better to use `@init` like:

```xml
<grid width="400px" model="@bind(vm.itemList)">
     <columns>
         <column label="index" />
         <column label="name" />
     </columns>
     <template name="model" >
        <row>
            <label value="@init(forEachStatus.index)" />
            <label value="@init(each.name)" />
        </row>
    </template>
</grid>

```


