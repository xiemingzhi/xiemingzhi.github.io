---
title: Jekyll Syntax Highlighting Example Part 1
---

The page.title is included in the layout template.

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
