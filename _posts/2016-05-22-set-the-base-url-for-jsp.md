---
title: "Set the base url for jsp"
description: ""
category: 
tags: [java, jsp]
---
{% include JB/setup %}

Sometimes when you're working on jsp pages and you need to set a base url for all pages, and this value needs to be available for use, then use the following

{% highlight javascript %} 
    <c:set var="req" value="${pageContext.request}" />
	<c:set var="uri" value="${req.requestURI}" />
	<c:set var="url">${req.requestURL}</c:set>
	<base href="${fn:substring(url, 0, fn:length(url) - fn:length(uri))}${req.contextPath}/" />
{% endhighlight %} 	

For css and js files you can use these kind of href:

{% highlight javascript %} 
    <link href="resources/bootstrap2/css/bootstrap.css" rel="stylesheet">
    <script type="text/javascript" src="resources/js/lib/jquery.js"></script>
{% endhighlight %} 	

This will work when it is deployed on different servers with different ports.
