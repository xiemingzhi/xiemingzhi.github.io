---
title: "Java Encapsulation Inheritance Polymorphism"
description: ""
category: 
tags: [java]
---
{% include JB/setup %}

What is encapsulation?  
Encapsulation can be used to hide implementation details but it can also be used to separate your application into logical parts.  
Find what varies and separate them from those that don’t change. For example Guitar and GuitarSpecs; GuitarSpecs varies so put in separate class.
Separate data and behaviour. class variables are data, setters and getters are behaviour. Hide implementation details.
SalesTax and CalcTax (USTax and CanTax).  

What is inheritance?
Inheritance is when a class is based on another class, using the same implementation to maintain the same behavior.  
Good for code reuse and to allow extensions of the original software.  

What is polymorphism?
Polymorphism is the provision of a single interface to entities of different types.  
Provide a single interface for computation.  

Encapsulation and Base class:

{% highlight java %}
public class Vehicle {
  private String brand;
  private String name;
  public VehicleSpecs;
  public Boolean isFlyable() {
    return false;
  }
  get/set methods
}

public class VehicleSpecs {
  private String factory;
  private String supplier;
  get/set methods;
}
{% endhighlight %}

Inheritance:

{% highlight java %}
public class Car extends Vehicle {
  private Integer numWheels;
  public Boolean isFlyable() {
    return false;
  }
  get/set methods
}
{% endhighlight %}

Inheritance:

{% highlight java %}
public class Aeroplane extends Vehicle {
  private Integer numWheels;
  public Boolean isFlyable() {
    return true;
  }
  get/set methods
}
{% endhighlight %}

Polymorphism:

{% highlight java %}
Vehicle vec1 = new Car();
Vehicle vec2 = new Aeroplane();

canYouFly(vec1);
canYouFly(vec2);

public void canYouFly(Vehicle vec) {
  System.out.println("vec name " + vec.getName() + " can you fly? ans = " + vec.isFlyable());  
}

{% endhighlight %}

{% highlight java %}
{% endhighlight %}
