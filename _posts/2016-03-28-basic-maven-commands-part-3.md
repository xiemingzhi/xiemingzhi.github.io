---
title: "Basic Maven Commands Part 3"
description: ""
category: 
tags: [maven]
---
{% include JB/setup %}

use jetty to run program  
To use the Jetty Plugin just add the following in your pom.xml:  
{% highlight xml %}
<project>
  ...
  <build>
    <plugins>
      <plugin>
        <groupId>org.mortbay.jetty</groupId>
        <artifactId>maven-jetty-plugin</artifactId>
        <version>6.1.10</version>
        <configuration>
          <scanIntervalSeconds>10</scanIntervalSeconds>
          <connectors>
            <connector implementation="org.mortbay.jetty.nio.SelectChannelConnector">
              <port>8080</port>
              <maxIdleTime>60000</maxIdleTime>
            </connector>
          </connectors>
        </configuration>
      </plugin>
      ...
    </plugins>
  </build>
  ...
</project>
{% endhighlight %}

Then start Jetty:  
{% highlight shell %}
  mvn jetty:run
{% endhighlight %}

Update:  
From [jetty maven plugin website](http://www.eclipse.org/jetty/documentation/current/jetty-maven-plugin.html)
{% highlight xml %}
<plugin>
  <groupId>org.eclipse.jetty</groupId>
  <artifactId>jetty-maven-plugin</artifactId>
  <version>9.3.7.v20160115</version>
</plugin>
{% endhighlight %}
Use the updated pluging.  

