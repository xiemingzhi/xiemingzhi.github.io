---
title: "Adding search to your blog"
description: ""
category: 
tags: []
---
{% include JB/setup %}

How to add search to blog

1. goto www.google.com/cse google custom search engine  
2. add your website  
3. do verification, put google html onto your website, click on verify.  
4. submit your sitemap https://xiemingzhi.github.io/sitemap.txt on google cse console  
5. retrieve code snippet from google cse console  
6. add code snippet to _includes/themes/bootstrap-3/default.html  
{% highlight html %}
		  <form action="http://www.google.com/cse" id="cse-search-box" target="_blank" class="navbar-form navbar-right" role="search">
			<input name="cx" type="hidden" value="your_cse_code_id" /> 
			<input name="ie" type="hidden" value="UTF-8" />
			<input name="q" size="30" />
			<input name="sa" type="submit" value="Search" /> 
		  </form>
{% endhighlight %}
