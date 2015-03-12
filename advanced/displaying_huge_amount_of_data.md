# Displaying Huge Amount of Data

The Huge Amount of Data Problem
===============================

When we display data with data components (*Listbox* or *Grid*), we may create a model object which contains all data for a data component. But sometimes the time to query all data from a database is unbearable long. Under this situation, we cannot put all data in the model object at one time. The model object should query a small group of data each time which are required by the component's rendering. We now demonstrate a custom `ListModel` implementation which only queries a small size of data a time instead of all data in this section.

Cache One Page Size of Data
===========================

Assume we are going to display large amount of personal information in a *Listbox*. Because there are too many of them, they cannot be displayed at one time. One way is set "rows" to limit the visible rows, and another way is to use "paging" mold.

``` xml
    <window  width="400px" apply="org.zkoss.bind.BindComposer"
        viewModel="@id('vm') @init('org.zkoss.reference.developer.mvvm.advance.HugeDataVM')">
         Use custom ListModel:
        <listbox model="@bind(vm.personListModel)" selectedItem="@bind(vm.selectedPerson)" rows="10">
            <listhead>
                <listheader label="ID" />
                <listheader label="First Name" />
                <listheader label="Last Name" />
                <listheader label="Age" />
            </listhead>
            <template name="model">
                <listitem>
                    <listcell label="@bind(each.id)" />
                    <listcell label="@bind(each.firstName)" />
                    <listcell label="@bind(each.lastName)" />
                    <listcell label="@bind(each.age)" />
                </listitem>
            </template>
        </listbox>
...
    </window>
```

As querying all data is time-consuming, we need to implement a custom [ListModel](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zul/ListModel.html) for the *Listbox*. This custom `ListModel` contains no data at the beginning, it queries data from a database until each time the *Listbox* request the data from it. Because we have limited visible rows, when a *Listbox* renders its *Listitem*s, it only gets part of data for those visible rows instead of all data. During one execution, *Listbox* will call `ListModel.getElememtAt(int index)` several times to get a range of data it requires for rendering. To avoid performing one query for each `getElementAt()` calling, we query a page size of data and store it as the execution's attribute at the first `getElementAt()` calling. In the subsequent callings, we just get the item from the cache without querying to a database. This implementation reduces query time drastically and user still can browse all data. Thus this implementation solves the problem and works fine under *Listbox*'s paging or default mold.

#### Live cached ListModel implementation
``` java
public class LivePersonListModel extends AbstractListModel<Person>{

    private PersonDao personDao;
    private int pageSize = 10;
    private final static String CACHE_KEY= LivePersonListModel.class+"_cache";

    public LivePersonListModel(PersonDao personDao){
        this.personDao = personDao;
    }

    public Person getElementAt(int index) {
        Map<Integer, Person> cache = getCache();

        Person targetPerson = cache.get(index);
        if (targetPerson == null){
            //if cache doesn't contain target object, query a page size of data starting from the index
            List<Person> pageResult = personDao.findAll(index, pageSize);
            int indexKey = index;
            for (Person o : pageResult ){
                cache.put(indexKey, o);
                indexKey++;
            }
        }else{
            return targetPerson;
        }

        //get the target after query from database
        targetPerson = cache.get(index);
        if (targetPerson == null){
            //if we still cannot find the target object from database, there is inconsistency between memory and the database
            throw new RuntimeException("Element at index "+index+" cannot be found in the database.");
        }else{
            return targetPerson;
        }
    }

    private Map<Integer, Person> getCache(){
        Execution execution = Executions.getCurrent();
        //we only reuse this cache in one execution to avoid accessing detached objects.
        //our filter opens a session during a HTTP request
        Map<Integer, Person> cache = (Map)execution.getAttribute(CACHE_KEY);
        if (cache == null){
            cache = new HashMap<Integer, Person>();
            execution.setAttribute(CACHE_KEY, cache);
        }
        return cache;
    }

    public int getSize() {
        return personDao.findAllSize();
    }

    ...
}
```
-   Line 1: We can create our `ListModel` class based on [AbstractListModel](http://www.zkoss.org/javadoc/latest/zk/org/zkoss/zul/AbstractListModel.html) in most cases without implementing from scratch.
-   Line 3: The `personDao` is a *Data Access Object* which queries data from a database.
-   Line 16-22: It queries one page size of data and put them into a cache object.
-   Line 37: This method gets cache object from execution's attribute. If no exist, put a new one.

