# Legacy Support (SimpleForm)

> Since 9.5.0

Before the introduction of Form Proxies in ZK a HashMap based SimpleForm was used, 
which was removed in ZK 8+. Upgrading to ZK 8/9 may be difficult in case of heavy 
customization/specialization of the related classes. In order to enable an upgrade
strategy, SimpleForm is reintroduced in ZK 9.5.0.

Below an overview of the related classes and availabilities in the affected version 
history.

## up to ZK 7.0.x

SimpleForm and related classes/interfaces

```java
org.zkoss.bind.Form
org.zkoss.bind.FormExt
org.zkoss.bind.impl.FormImpl
org.zkoss.bind.SimpleForm
```
## ZK 8.x, 9.0.x, 9.1.x

```java
org.zkoss.bind.Form  /* base interface for Form Proxies*/
```

`org.zkoss.bind.SimpleForm` is not available in these versions.

## Since ZK 9.5.0

Updated class hierarchy enabling both Form Proxies and SimpleForm.

```java
org.zkoss.bind.Form
/* re-added / deprecated / available for ZK 7 compatibility */
org.zkoss.bind.FormLegacy
org.zkoss.bind.FormLegacyExt
org.zkoss.bind.impl.FormImpl
org.zkoss.bind.SimpleForm
```

### Upgrade/refactor steps:

**Renamed interfaces**

```java
org.zkoss.bind.Form -> org.zkoss.bind.FormLegacy
org.zkoss.bind.FormExt -> org.zkoss.bind.FormLegacyExt
```

**SimpleForm Usage**

The default usage from a zul file will still create a `FormProxyObject` (default since ZK 8.0.x)

```xml
<div form="@id('fx') @load(...) @save(...)">
```

Instead, a `SimpleForm` can be initialized from zul code (compatible to ZK 7 - without changing Java code) using the 
[TLD function `c:new`](https://www.zkoss.org/wiki/ZUML%20Reference/EL%20Expressions/Core%20Methods/new).

```xml
<?taglib uri="http://www.zkoss.org/dsp/web/core" prefix="c"?>
...
<div form="@id('fx') @init(c:new('org.zkoss.bind.SimpleForm')) @load(...) @save(...)">
```

As in ZK 7 a pre-created `SimpleForm` instance can be accessed from a view model.

```xml
<div form="@id('fx') @init(vm.myForm) @load(...) @save(...)">
```
