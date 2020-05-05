---
title: "Basic Maven Commands"
description: ""
category: 
tags: []
---
{% include JB/setup %}

maven commands to run program
{% highlight shell %}
mvn exec:java -Dexec.mainClass="com.example.Main"
{% endhighlight %}

maven commands to run program with args
{% highlight shell %}
mvn exec:java -Dexec.mainClass="com.example.Main" -Dexec.args="arg1 arg2"
{% endhighlight %}

maven commands to run test
{% highlight shell %}
mvn test -Dtest=com.example.MainTest
{% endhighlight %}

maven commands to run single test
{% highlight shell %}
mvn test -Dtest=com.example.MainTest#testMethod1 
{% endhighlight %}

{% highlight shell %}
{% endhighlight %}