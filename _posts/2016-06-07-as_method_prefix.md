---
layout: post
title: "as in method names"
description: ""
category: 
tags: [methods, ]
---
{% include JB/setup %}

What is the difference between a method whose name starts with `to`, versus a a method starting with `as`? For instance, what is the difference between a `toMap()` and a `asMap()` method?

The main difference has to do with the expectations when calling the method. When a method has a `to` prefix, I expect that method converts its internal data into a representation that is returned. This representation has no relation with the original object: the two are **detached**.

When a method has a `as` prefix, I expect that method to expose a view on its internal data. The view is **connected** to the original, in the sense that any changes made on it will also be reflected in the original. And the reverse is also true: any changes made to the original will also be reflected to the view.

There are plenty of examples of this naming pattern to be found in the JDK. For instance, consider [`Collection.toArray()`][toArray], where the Javadoc clearly states that there is not connection between the original collection and the returned array:

> The returned array will be "safe" in that no references to it are maintained by this collection.

Compare that to  [`Arrays.asList(T[])`][asList], where the documentation states:

>Changes to the returned list "write through" to the array.

We can also show the difference in a little program. First, using `asList`. You will notice that any change made to the list will reflect the source array, and vice-versa:

```java
String[] array = new String[]{"foo", "bar"};
List<String> list = Arrays.asList(array);

list.set(1, "baz"); // change the view
System.out.println(Arrays.asList(array)); // prints [foo, baz]
System.out.println(list); // [foo, baz]

array[1] = "qux"; // change the original
System.out.println(Arrays.asList(array)); // [foo, qux]
System.out.println(list); //[foo, qux]
```

whereas, with `toList()`, you will see that changes in one are not reflected in the other:

```java
List<String> list = Arrays.asList("foo", "bar");
String[] array = list.toArray(new String[2]);

array[1] = "baz"; // change the conversion
System.out.println(list); // [foo, bar]
System.out.println(Arrays.asList(array)); // [foo, baz]

list.set(1, "qux"); // change the original
System.out.println(list); //[foo, qux]
System.out.println(Arrays.asList(array)); // [foo, baz]
```

So if you start your method name with `as`, make sure that the return value has a connection to the original. Otherwise, you are setting up expectations that will not be met.

[asList]: https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#asList-T...-	"Arrays.asList"
[toArray]: https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#toArray--	"Collection.toArray"

