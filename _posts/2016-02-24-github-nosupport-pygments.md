---
layout: post
title: Github does not support pygments
---


If you are blogging using github and are using syntax highlighting then watch out for this.<br>
Github does not support pygments anymore.<br>
<p>
You will get this email:<br>
You are attempting to use the 'pygments' highlighter, which is currently unsupported on GitHub Pages. Your site will use 'rouge' for highlighting instead. To suppress this warning, change the 'highlighter' value to 'rouge' in your '_config.yml'. For more information, see https://help.github.com/articles/page-build-failed-config-file-error/#fixing-highlighting-errors.
</p>

Change your _config.yml to this:<br>
{% highlight yaml %}
safe: true
lsi: false
highlighter: rouge 
{% endhighlight %}


