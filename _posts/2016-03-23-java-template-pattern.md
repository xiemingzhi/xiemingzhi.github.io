---
title: "Java Template pattern"
description: ""
category: 
tags: [java]
---
{% include JB/setup %}

The Template pattern formalizes the idea of defining an algorithm in a class, but leaving some of the details to be implemented in subclasses.

Reasons for using Template pattern:

Let subclasses implement behavior that can vary.  
Avoid duplication in the code, the base class contains the common code.  
Control at what points subclassing is allowed, because the basic workflow are defined in the template method you are specifying which methods are allowed to change.

Template pattern:

{% highlight java %}
public abstract class MyProgram {
  public String name;
  public Connection conn;
  
  public abstract void initProgram();
  private final waitForResources() {
    //wait for network/db connections etc.
  }
  public abstract void processData();
  
  //Template method cannot be overriden
  public final void startProgram() {
    initProgram();
	waitForResources();
	processData();
  }
}
{% endhighlight %}

How to use the base class:

{% highlight java %}
public class RunProgram extends MyProgram {
  public void initProgram() {
    this.name = "FirstProgram";
	this.conn = getConnection();
  }
  public void processData() {
    //Do something to the data
  }
}
{% endhighlight %}

{% highlight java %}
{% endhighlight %}
