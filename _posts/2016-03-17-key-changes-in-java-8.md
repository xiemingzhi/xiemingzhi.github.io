---
title: "Key changes in Java 8"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Here are some new features inside Java 8

Lambda expressions - provides the ability to pass code as data to methods

{% highlight java %}
...
	public static void main(String[] args) {
		//take two values add return value
		MathOperation myadd = (a,b) -> (a+b);
		performAction(1, 2, myadd);
		//code as data
		MathOperation mymul = (a,b) -> (a*b);
		performAction(2, 2, mymul);
	}

	public static void performAction(int a, int b, MathOperation mo) {
		System.out.println("result = " + mo.operate(a, b));
	}
	
	interface MathOperation {
		int operate(int a, int b);
	}
...
{% endhighlight %}

Also makes code easier to read for things like anonymous classes. Classes that implement an interface with one method.

Streaming API - provide your data as a stream and use streaming functions to process the data. 

{% highlight java %}
...
		List<Integer> list = new ArrayList<Integer>(Arrays.asList(1,2,3));
		Integer result = list.stream().reduce(0, (a,b) -> (a+b));//0 identity and default value
		System.out.println("result = " + result);
		result = list.stream().mapToInt(Integer::intValue).sum();
		System.out.println("result = " + result);
...		
{% endhighlight %}

This also allows the JVM better use of multi processor CPUs. Better concurrency processing.

New DateTime API - immutable Date Time Objects with better API calls and time operations.

{% highlight java %}
...
		//To create Date objects you use of
		LocalDate bdate = LocalDate.of(1977, Month.JULY, 30);
		//You can use special functions to get more information about the Date
		DayOfWeek bdayWeek = bdate.getDayOfWeek();
		System.out.println("local date dayofweek = " + bdayWeek);
...
		//To calculate Period between two Dates use Period(Y,M,D)
		LocalDate lastLogin = LocalDate.of(2016, Month.FEBRUARY, 1);
		LocalDate loginDate = LocalDate.now();
		Period period = Period.between(lastLogin, loginDate);
		System.out.println("local Period months = " + period.getMonths());
		System.out.println("local Period days = " + period.getDays());
		System.out.println("local total days = " + ChronoUnit.DAYS.between(lastLogin, loginDate));
...
		//Show time in different zone and also add some time
		ZonedDateTime zonedLocalTime = ZonedDateTime.now();
		ZoneId laZone = ZoneId.of("America/Los_Angeles"); 
		ZonedDateTime laTime = zonedLocalTime.withZoneSameInstant(laZone);
		System.out.println("zoned localTime = " + zonedLocalTime);
		System.out.println("zoned LA time = " + laTime);
		ZonedDateTime arrivalTime = laTime.plusMinutes(650);
		System.out.println("zoned Arrival LA time = " + arrivalTime);
{% endhighlight %}

TODO: Optional class

{% highlight java %}
{% endhighlight %}
