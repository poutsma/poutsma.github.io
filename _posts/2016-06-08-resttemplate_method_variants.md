---
layout: post
title: "Why does RestTemplate have three overloaded methods for each HTTP operation?"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Looking at Spring's [`RestTemplate`][RestTemplate] class, you might wonder why it has three overloaded methods for each HTTP operation. For instance, consider `delete`:

- `detete(String, Object...)`
- `delete(String, Map<String, ?>)`
- `delete(URI)`

You will see these three variants throughout RestTemplate: one method that takes a string as first parameter and an array as last, one having taking a string as first and a map as last , and one taking just a `java.net.URI`.

The first variant is probably the most popular, as it allows you to easily expand uri variables as a varargs array:

```java
restTemplate.delete("http://example.com/{path}", "foo");
```

The second variant allows you to expand the same variable twice:

```java
Map<String, ?> vars = Collections.singletonMap("path", "foo");
restTemplate.delete("http://example.com/{path}/{path}", vars);
```

But perhaps the most interesting one is the final one, taking a URI:

```java
restTemplate.delete(new URI("http://example.com/foo"));
```

Why do we need an additional method for specifying a URI? Surely we could have used the first, varargs-based variant for that? Something like the following also works fine:

```java
restTemplate.delete("http://example.com/foo");
```

The reason behind the URI-based variant has to do with URI encodings. If you have string that represent a URI, there is no way to determine if that URI has already been encoded or not. So you run the risk of *double-encodings*. For instance, the following request

```java
restTemplate.delete("http://example.com/foo%20bar");
```
probably aims to perform a DELETE request to `http://example.com/foo bar`, but instead would connect to `http://example.com/foo%2520bar`. The`%` character is considered "dangerous" in a path, so it is encoded into `%25`.

We use the URI-based variant to solve the issue of double-encodings. In the Javadoc of `URI`, we can read that:

> The [single-argument constructor][uri-constructor] requires requires any illegal characters in its argument to be quoted and preserves any escaped octets and other characters that are present.


So by using a `URI` instead of a `String` as parameter, we can be sure that the URI has already been encoded, and won't do it twice.

[RestTemplate]: http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/client/RestTemplate.html	"RestTemplate Javadoc"
[uri-encodings]: https://blogs.msdn.microsoft.com/oldnewthing/20100331-00/?p=14443	"The great thing about URL encodings is that there are so many to choose from"
[uri-constructor]: https://docs.oracle.com/javase/8/docs/api/java/net/URI.html#URI-java.lang.String-	"URI(String) constructor"