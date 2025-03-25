---
layout: post
title: "Fluent APIs to forego illegal state"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Some time ago, Michael Nygard [tweeted](https://twitter.com/mtnygard/status/710051345324228608) the following:

> Hypothesis: Fluent API builders in Java are just a substitute for better data literal syntax.

To which I [responded](https://twitter.com/poutsma/status/710066037270642688):

> @mtnygard A good fluent API forces the user along the happy path, to forego illegal state. That’s not something you can do with syntax alone.

The 140 characters that Twitter offer are hardly enough to explain what I mean, so I will use this post to elaborate.

Let me start off that I find many uses of the `IllegalStateException` to be a *cop out*. Unless the exception contains clear instructions as to how to fix the illegal state, it is about as useful as a typical Windows dialog box:

![Outlook express]({{ site.url }}/assets/outlook_dialog.gif)

And even if the exception did contain instructions, preventing it from occurring is always better than helping your users correct their code once it occur. **The best exception is no exception at all**.

Second, I have found that many Java builders do not have fluent APIs. For me, in order to have a fluent API, a builder needs a lot more than method changing alone; it needs Method chaining does not a fluent API make.