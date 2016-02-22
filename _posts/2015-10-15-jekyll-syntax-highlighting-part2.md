---
layout: post
title: Jekyll Syntax Highlighting Example Part 2
---

{{ page.title }}
================

To check out how to do syntax highlighting see the source for this file [RAW](https://raw.githubusercontent.com/xiemingzhi/xiemingzhi.github.io/master/_posts/2015-10-15-jekyll-syntax-highlighting-part2.md).
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

