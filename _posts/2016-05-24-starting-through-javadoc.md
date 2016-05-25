---
layout: post
title: "Starting through Javadoc"
subtitle: "You never get a second chance to make a first impression"
description: ""
category: 
tags: []
---
{% include JB/setup %}

# Starting through API documentation

When someone is learning about your framework or library, they will often do so via API documentation.
In the API documentation, you will document the public types that make up your API.
If you have a lot of API, like we do in the [Spring Framework](http://projects.spring.io/spring-framework/), you will end up with a lot of documentation.

But not all the documentated types are equally important for people just getting started; new users typically only care about one or two central classes or interfaces.
And within the API documentation of those types, there will be one or two methods or constructors that form the main entry point to that class.
These central types and methods are the *doorway* into your API. 
The rest of the classes are typically related to these doorway types, in so far that they might customize their behaviorsomehow, so you will only need to know about them later, if at all.

![Many doors]({{ site.url }}/assets/many_doors.jpg)

But how do you make these special doorways stand out among their peers?

In Spring Framework, we solve this problem quite simply, by making the doorways **bold** in Javadoc.
[This][jdbc-core], for instance, is the package description of `org.springframework.jdbc.core`.
It is an old package, containing arguably too many types, and yet it remains quite easy to find the central class that 99% of the people care about: `JdbcTemplate`. We do this in other places too, for instance [in Spring Web Services][sws-client-core].

[jdbc-core]: http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/jdbc/core/package-summary.html "Spring Jdbc Core"
[sws-client-core]: http://docs.spring.io/spring-ws/docs/2.3.0.RELEASE/api/org/springframework/ws/client/core/package-summary.html "Spring WS Client Core"


