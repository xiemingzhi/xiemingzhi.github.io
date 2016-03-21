---
layout: post
title: "Java Overriding vs Overloading"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Method overloading, is when method with same name co-exists in same class but they must have different method signature.

1. Method overloading is done via static binding.  

Method overriding, is when method with same name is declared in derived class or sub class.

1. Method **signature must be same** including return type, number of method parameters, type of parameters and order of parameters
2. Overriding method **can not throw higher Exception** than original or overridden method. This rule only applies to checked Exception in Java, overridden method is free to throw any unchecked Exception.
3. Overriding method **can not reduce accessibility** of overridden method i.e if original or overridden method is public than overriding method can not make it protected.
4. Private and Final methods cannot be overridden.
5. Method overriding is done via dynamic binding, therefore static methods cannot be overridden.

{% highlight java %}
{% endhighlight %}
