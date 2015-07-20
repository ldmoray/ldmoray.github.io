---
layout: post
title:  "Python: Context Managers, Man"
date:   2015-07-20 13:26:43
categories: python
---
Context managers are awesome.

{% highlight python %}
with open('file.txt') as f:
    for line in f:
        print line
{% endhighlight %}

When you enter the block (though it doesn't define scope), f exists and is opened for you. When you leave the block, it automatically closes the file for you. It basically replaces "try/finally", and they're super easy to write yourself. In a class, you either define `__enter__()` and `__exit__()`,  or use the decorator in [contextlib] to turn a simple function with a `yield` into one.

[contextlib]: https://docs.python.org/2/library/contextlib.html
