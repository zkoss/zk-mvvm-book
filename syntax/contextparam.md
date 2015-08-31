# @ContextParam

Syntax
======

``` java
@ContextParam(ContextType.PAGE)
```

#### Enumeration of all context
``` java
enum ContextType {
    BIND_CONTEXT,   //BindContext instance
    BINDER,         //Binder instance
    TRIGGER_EVENT,  //Event that trigger the command (since 6.0.1)
    COMMAND_NAME,   //Command name (since 6.0.1)
    EXECUTION,      //Execution instance
    COMPONENT,      //Component instance of current binding
    SPACE_OWNER,    //IdSpance instance of spaceOwner of current component
    VIEW,           //the view component of binder
    PAGE,           //Page instance of current component
    DESKTOP,        //Desktop instance of current component
    SESSION,        //Session instance
    APPLICATION     //Application instance
}
```

Description
===========

**Target:** A method's parameter (for initial and command methods)

**Purpose:** Tell binder to pass the context object with specified type.

The annotation is applied to initial (or command) method's parameter. Methods can get various ZK context object like: Page or Desktop by applying annotation on parameters.

Example
=======

#### Retrieve various context object in a ViewModel
``` java
@Init
public void init(
    @ContextParam(ContextType.EXECUTION) Execution execution,
    @ContextParam(ContextType.COMPONENT) Component component,
    @ContextParam(ContextType.VIEW) Component view,
    @ContextParam(ContextType.SPACE_OWNER) IdSpace spaceOwner,
    @ContextParam(ContextType.PAGE) Page page,
    @ContextParam(ContextType.DESKTOP) Desktop desktop,
    @ContextParam(ContextType.SESSION) Session session,
    @ContextParam(ContextType.APPLICATION) WebApp application,
    @ContextParam(ContextType.BIND_CONTEXT) BindContext bindContext,
    @ContextParam(ContextType.BINDER) Binder binder) {

    // method body
}
```

The following is another example.

``` xml
<vbox id="vbox" apply="org.zkoss.bind.BindComposer" viewModel="@id('vm') @init('eg.ContextParamVM')">
    <button id="cmd" label="cmd" onClick="@command('cmd')"/>
</vbox>
```

#### A ViewModel used by above zul
``` java
public class ContextParamVM {
    @Command
    public void cmd(
        @ContextParam(ContextType.COMPONENT) Component component,
        @ContextParam(ContextType.VIEW) Component view) {

        // method body
    }
}
```

-   In above example, the variable component is a Button object and view is a Vbox object.

