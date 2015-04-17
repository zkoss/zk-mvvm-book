# Wire Components
> Since 6.0.2

Although the design principle of MVVM pattern is that **ViewModel should not have any reference to UI components, ZK still provides two ways to retrieve UI components in a ViewModel**. However, we do not suggest this usage as it loses ViewModel an important advantage: loose coupling with View. **Notice that binder also manipulates UI components, so your operation to UI components may affect binder's work. Please be careful when using it**.

One way to get components is [passing components as parameters in command binding](./parameters.html) which we have talked before. Another is to call `Selectors.wireComponents()` . This way enables you to wire components with `@Wire` like what you do in a [SelectorComposer](http://books.zkoss.org/wiki/ZK_Developer%27s_Reference/MVC/Controller/Wire_Components). You should call `Selectors.wireComponents()` in a method with `@AfterCompose` as follows:

#### Example to wire components in a ViewModel
```java
public class SearchAutowireVM{

    //UI component
    @Wire("#msgPopup")
    Popup popup;
    @Wire("#msg")
    Label msg;

    @AfterCompose
    public void afterCompose(@ContextParam(ContextType.VIEW) Component view){
        Selectors.wireComponents(view, this, false);
    }

}
```
* `Selectors.wireComponents()`'s first parameters is Root View Component which can be retrieved by `@ContextParam`
