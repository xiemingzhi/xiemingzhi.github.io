---
title: Using Spring Framework Inside Google App Engine
---


Trying to port [spring project](https://github.com/xiemingzhi/springwebapp) to run on google app engine came across some stumbling blocks. The following is a list of things that didn't worked and one that worked.

Tried HSQL database did not work, running the same program but with an embedded database inside gae, but found out that you couldn't because gae doesn't allow you write files to disk.

Tried Spring boot + Spring data inside gae did not work, used a [spring-boot-gae-example](https://github.com/making/spring-boot-gae-blank) added spring data jpa to it, problem could not find how to connect to datasource
got the following exception:
<pre>
[INFO] Caused by: org.springframework.beans.BeanInstantiationException: Failed t
o instantiate [org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBea
n]: Factory method 'entityManagerFactory' threw exception; nested exception is j
ava.lang.NoClassDefFoundError: javax.management.NotificationBroadcasterSupport i
s a restricted class. Please see the Google  App Engine developer's guide for mo
re details.
</pre>

Tried Spring core + Spring data + Datanucleus inside gae did not work, change the program to spring core because couldn't see what spring boot was doing behind the seens, had to add [datanucleus](www.datanucleus.org/products/accessplatform_4_0/getting_started.html) in your pom.xml as a plugin to enhance your classes for google cloud datastore, problem could not get datanucleus:enhance to work.

Tried Spring core + Google Cloud Datastore inside gae partial work, rewrote the program i.e the DAOs to use the [datastore api](https://cloud.google.com/appengine/docs/java/datastore/), problem could insert data but could not retrieve it probably because didn't register the classes.

Tried Spring core + Objectify inside gae working, rewrote the program again this time using the [objectify api](https://objectify-appengine.googlecode.com/git-history/2.2.1/javadoc/index.html), also had to create a helper class to register the classes inside the servletcontextlistener, got both web interface and restful interface working.

{% highlight java%}
public class OfyHelper implements ServletContextListener {
	  public void contextInitialized(ServletContextEvent event) {
	    // This will be invoked as part of a warmup request, or the first user request if no warmup
	    // request.
	    ObjectifyService.register(Contact.class);
	  }

	  public void contextDestroyed(ServletContextEvent event) {
	    // App Engine does not currently invoke this method.
	  }
}
{% endhighlight %}

Add the listener to web.xml
{% highlight xml %}
<filter>
  <filter-name>ObjectifyFilter</filter-name>
  <filter-class>com.googlecode.objectify.ObjectifyFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>ObjectifyFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
<listener>
  <listener-class>com.mycompany.OfyHelper</listener-class>
</listener>
{% endhighlight %}

Use the ObjectifyService to perform your CRUD functions:
{% highlight java %}
@Component("objectifyDAO")
public class ContactDAOObjectify implements ContactDAO {
	ObjectifyService os;
	
	public void insertContact(Contact contact) {
		ObjectifyService.ofy().save().entity(contact).now();

	}

	public List getContacts() {
		List<Contact> contacts = ObjectifyService.ofy()
		          .load()
		          .type(Contact.class) // We want only Contact
		          .list();
		System.out.println("objectify number of contacts is "+contacts.size());
		return contacts;
	}

	public Contact getContact(Long contactId) {
		Contact c = ObjectifyService.ofy().load().type(Contact.class).id(contactId).now();
		return c;
	}

	public void updateContact(Contact contact) {
		ObjectifyService.ofy().save().entity(contact).now();

	}

	public void deleteContact(Long contactId) {
		Contact c = ObjectifyService.ofy().load().type(Contact.class).id(contactId).now();
		ObjectifyService.ofy().delete().entity(c).now();
	}
}
{% endhighlight %}