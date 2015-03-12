# Wire Event Listeners

> since 6.0.2

To wire event listeners in a ViewModel like [ZK Developer's Reference/MVC/Controller/Wire Event Listeners](http://books.zkoss.org/wiki/ZK_Developer%27s_Reference/MVC/Controller/Wire_Event_Listeners), we have to call ` Selectors.wireEventListeners() ` in a method with `@AfterCompose`. We then can use ` @Listen` to declare a method as an event listener. **We do not recommend this usage** because it loses <b>ViewModel</b> an important advantage: loose coupling with View. **Please evaluate the trade-offs before using it.**

#### Wire event listener in a ViewModel
``` java

public class SearchAutowireVM{

    @AfterCompose
    public void afterCompose(@ContextParam(ContextType.VIEW) Component view){
        Selectors.wireEventListeners(view, this);
    }

    @Listen("onClick=#mybutton")
    public void submit(MouseEvent event){
        //handle events
    }
}
```

-   ` Selectors.wireEventListeners()` 's first parameters is Root View Component which can be retrieved by `@ContextParam`.
