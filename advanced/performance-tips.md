# Performance Tips

## Choose wisely among data bindings

`@init`, `@load`, `@save`, and `@bind` are mostly common used annotations. But `@init` will load the ViewModel's value once and don't track the value which is different from other annotations. ZK will create tracking nodes in memory for those bindings except `@init`, and those tracking nodes consume a server's memory.

By expensiveness in memory, they are ranked as follow: `@init` < `@load` | `@save` < `@bind`.

To save a server's memory, use `@init` if you don't need to reload a value when it's changed. A binding to an object of a collection doesn't need to be reloaded for displaying data only, so it's better to use `@init` like:

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


