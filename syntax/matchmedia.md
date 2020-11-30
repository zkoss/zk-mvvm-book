# @MatchMedia
> Since 8.0.2

Syntax
======

``` java
@MatchMedia("mediaQuery")

@MatchMedia({"mediaQuery1", "mediaQuery2"})
```

Description
===========

**Target:** method

**Purpose:** For Responsive Web Design (RWD). To identify a MatchMedia method that will be invoked when matched with a media query.

**Note:** Values of this annotation should be unique in each ViewModel, and have to follow the criteria listed in <a href="http://www.w3schools.com/cssref/css3_pr_mediaquery.asp">this media query's spec</a>.

A <a href="https://www.zkoss.org/javadoc/latest/zk/org/zkoss/zk/ui/event/ClientInfoEvent.html">ClientInfoEvent</a> will be sent when the media query is matched, and we will be able to get the client's information by using [ ContextParam](./contextparam.html) parameter. (see example line 5, line 8)

Other parameter annotations could also be used; please refer to subsections of [ Parameters](./parameters.html) for more information.

To make sure a MatchMedia method that will be invoked when matched with a media query as expected, It is recommended that you should define matchMedia more clearly. 

For example: There are two MatchMedia method @MatchMedia("all and (min-width: 300px)") and @MatchMedia("all and (min-width: 600px)"), the @MatchMedia("all and (min-width: 300px)") method won't be invoked when width became 599px from 600px. if you want to do stuff between 300px~599px, it's better to use @MatchMedia("all and (min-width: 300px) and (max-width: 599px)").

Example
=======

``` java
String windowWidth = "700px";
String applyTemplateName = "min501";

// getter setter

@MatchMedia("all and (max-width: 500px)")
@NotifyChange("*")
public void max500(@ContextParam(ContextType.TRIGGER_EVENT) ClientInfoEvent event) {
	windowWidth = "100%";
    applyTemplateName = "max500"; //change a template
	int desktopWidth = event.getDesktopWidth(); // get the desktop width by the event
	// do something according to desktopWidth
}

@MatchMedia("all and (min-width: 501px)")
@NotifyChange("*")
public void min501() {
	windowWidth = "700px";
    applyTemplateName = "min501"; //change a template
}
```

```xml
<div viewModel="@id('vm') @init('org.zkoss.VM')">
	<window width="@bind(vm.windowWidth)">
		<apply template="@bind(vm.applyTemplateName)"/>
	</window>
</div>
<template name="min501">
	//zk components
</template>
<template name="max500">
	//zk components
</template>
```
