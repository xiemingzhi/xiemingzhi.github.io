---
title: Spring Framework ViewResolver
---


My [spring project](https://github.com/xiemingzhi/springwebapp) needed to have two access points http://localhost:8080/springwebapp/ and http://localhost:8080/springwebapp/contact/jsonlist.json one for web access one for RESTful access.

To do this change your viewresolver to org.springframework.web.servlet.view.ContentNegotiatingViewResolver then use org.springframework.web.servlet.view.InternalResourceViewResolver for web access and org.springframework.web.servlet.view.json.MappingJackson2JsonView for RESTful access.

{% highlight xml %}
<bean id="viewResolver" class="org.springframework.web.servlet.view.ContentNegotiatingViewResolver">
	    <property name="viewResolvers">
	        <list>
	            <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	            	<property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
	                <property name="prefix" value="/jsp/"/>
	                <property name="suffix" value=".jsp"/>
	            </bean>
	        </list>
	    </property>
	    <property name="defaultViews">
	        <list>
	            <bean class="org.springframework.web.servlet.view.json.MappingJackson2JsonView"/>
	        </list>
	    </property>
    </bean>
{% endhighlight %}

Add or update this in your spring configuration (springwebapp-servlet.xml). To use MappingJackson2JsonView you have to add the fasterxml dependency into pom.xml:

{% highlight xml %}
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.4.1.3</version>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-annotations</artifactId>
			<version>2.4.1</version>
		</dependency>
{% endhighlight %}

