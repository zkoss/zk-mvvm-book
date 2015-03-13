# Data Binding

In the following sections, we'll cover ZK Bind's annotations used on a ZUL. They are all used in **component's attribute** and have general format described in [ Annotate in ZUML](http://books.zkoss.org/wiki/ZK_Developer%27s_Reference/Annotations/Annotate_in_ZUML):

**@AnnotationName(*attr-value1, attr-name2=attr-value2*)**

**@AnnotationName(*attr-name1={attr-value1-1, attr-value1-2}, attr-name2=attr-value2*)**

Basically, the syntax consists of an annotation name with comma-separated key-value pairs. The binder treats an annotation's attribute value as an EL expression, and sets an EL expression without a attribute name to default attribute name "value". All annotations must define default attribute's value.

For each ZK Bind supported annotation, the way a binder evaluates an EL expression is slightly different. Some annotation's EL expressions are evaluated only once like `@id` and `@init`. some annotations have reserved attribute names, "before" and "after", like `@load` and `@save`.
