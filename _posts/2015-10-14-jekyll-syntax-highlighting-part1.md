---
layout: post
title: Jekyll Syntax Highlighting Example Part 1
---

{{ page.title }}
================

<p>
This is an example of syntax highlighting.<br>
Github does not support rouge so have to change to Pygments.<br>
Also change to index.html because have to include css.<br>
</p>

{% highlight java %}
public class Hello {
	public static void main(String args[]) {
		System.out.println("Hello World");
	}
}
{% endhighlight %}
