# &lt;forEach>
Syntax
======
```xml
<forEach begin="0" end="3" step="2" var="x">
  ...
</forEach>
```

```xml
<forEach items="@load(vm.model)" >
  <label value="@load(each)"/>
</forEach>
```

Description
===========
**Purpose:**

The basic iteration tag, accepting many different collection types and supporting sub-setting and other functionality like JSTL's `forEach` .

Specifying the collection by using the **items** attribute, and the current item is available through a variable named by the **var** attribute.


Example
=======
```xml
<forEach begin="@load(vm.begin)" end="@load(vm.end)" step="@load(vm.step)" var="x">
	<label value="${x}"/>
</forEach>
```

```xml
<forEach items="@load(vm.model)">
	<label value="@load(each)"/>
</forEach>
```

varStatus
=======

The varStatus can be declared on forEach to access the status object for each iteration.
the [forEachStatus interface](https://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/util/ForEachStatus.html) can be used to retrieve iteration relevant statuses, such as "first", "last" or the current index, start and end indexes, etc.

```xml
<zk>
	<div apply="org.zkoss.bind.BindComposer">
		<zscript>
	  		ListModelList model = new ListModelList();
	      	model.add("foo");
	      	model.add("bar");
	      	model.add("baz");
			model.add("qux");
	  	</zscript>
	  	per model
		<hlayout>
		  	<forEach items="@load(model)" var="item" varStatus="itemStatus">
	      		<vlayout>    
					<label value='@init(("item: " += item))'/>
			    	<label value='@init(("begin: " += itemStatus.begin))'/>
			    	<label value='@init(("count: " += itemStatus.count))'/>
			    	<label value='@init(("current: " += itemStatus.current))'/>
			    	<label value='@init(("end: " += itemStatus.end))'/>
			    	<label value='@init(("index: " += itemStatus.index))'/>
			    	<label value='@init(("step: " += itemStatus.step))'/>
			    	<label value='@init(("first: " += itemStatus.first))'/>
			    	<label value='@init(("last: " += itemStatus.last))'/>
			    </vlayout>
	  		</forEach>
	  	</hlayout>
	  	<separator orient="horizontal" />
	 	per iteration
	  	<hlayout>
	  		<forEach begin="4" end="10" step="2" var="item" varStatus="itemStatus">
	      		<vlayout>    
					<label value='@init(("item: " += item))'/>
			    	<label value='@init(("begin: " += itemStatus.begin))'/>
			    	<label value='@init(("count: " += itemStatus.count))'/>
			    	<label value='@init(("current: " += itemStatus.current))'/>
			    	<label value='@init(("end: " += itemStatus.end))'/>
			    	<label value='@init(("index: " += itemStatus.index))'/>
			    	<label value='@init(("step: " += itemStatus.step))'/>
			    	<label value='@init(("first: " += itemStatus.first))'/>
			    	<label value='@init(("last: " += itemStatus.last))'/>
			    </vlayout>
	  		</forEach>
		</hlayout>
	</div>
</zk>
```