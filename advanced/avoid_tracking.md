# Avoid Tracking
When we create a property binding to a ViewModel's property, a **tracker** creates a corresponding tracking record to maintain this binding relationship. Thus when a binder reads properties from annotation ` @NotifyChange `, it knows which attributes to reload upon the tracking records. This tracking task consumes time and memory and impacts application performance. If we have an object whose properties never change during whole application running, it's unnecessary to keep track of this immutable object's properties. We can apply ` @Immutable ` annotation on this immutable object to reduce the cost of tracking. If we bind a attribute to an immutable object's property, the tracker won't create a corresponding tracking record for it.

#### Immutable object
``` java
@Immutable
public class SysDefaultConfig {
	// class body
}
```

#### Reference an immutable object
``` xml
<label value="@init(vm.sysDefaultConfig.size)"/>
```
