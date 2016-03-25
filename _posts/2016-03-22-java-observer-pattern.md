---
layout: post
title: "Java Observer pattern"
description: ""
category: 
tags: [java]
---
{% include JB/setup %}

Observer pattern - one subject many observers, state changes in the subject must be notified to all observers.  
Observers first register with the subject, once the states the subject calls the notify method in the registered observers.

{% highlight java %}
public class NotificationService {
  public List<Listener> listeners;
  public void notifyListeners() {
    for (Listener listener : listeners) {
	  listener.notify();
	}
  }
}
{% endhighlight %}

{% highlight java %}
public interface Listener {
  public void notify();
}
{% endhighlight %}

{% highlight java %}
public class MyService implements Listener {
  public void notify() {
    //when i receive a message i do something
	//for example notify my app
  }
}
{% endhighlight %}

Register MyService:

{% highlight java %}
NotificationService notifyService = new NotificationService();
MyService theService = new MyService();
notifyService.getListeners().add(theService);

{% endhighlight %}

{% highlight java %}
{% endhighlight %}
