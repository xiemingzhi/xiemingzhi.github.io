---
title: "AngularJS project to call RESTful service part 2"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Using [ang-starter](https://github.com/mriverodorta/ang-starter/) to create sample AngularJS app. The structure is easily understandable and is using a later version of angular.

Modified the js/Controllers/ContactCtrl.js, calling the RESTful service using $http.

TODO: Change to use 'ngResource'

Modified partials/contact.html to display the result using a simple table.

TODO: Change project to 'Angular Material Design'

The 'app' directory is the deliverable, use apache to host the files. Add apache config settings to httpd.conf.

Use your browser to view results, open dev view, watch the console tab. You may encounter "no 'access-control-allow-origin' header is present on the requested resource" this means your RESTful resource is not allowing CORS. Enable this to springwebapp by adding a filter.

{% highlight java %}
public class SimpleCORSFilter implements Filter {
...
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
			HttpServletResponse resp=(HttpServletResponse) response;
					 
					 		resp.setHeader("Access-Control-Allow-Origin", "*");
									resp.setHeader("Access-Control-Allow-Methods", "POST, GET, PUT, DELETE");
											resp.setHeader("Access-Control-Max-Age", "3600");
													resp.setHeader("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
															// pass the request along the filter chain
																	chain.doFilter(request, response);
																		}
																		...
																		}

{% endhighlight %}

Specify the filter inside web.xml

{% highlight xml %}
<filter>
    <filter-name>simpleCORSFilter</filter-name>
        <filter-class>
	        com.mycompany.filter.SimpleCORSFilter
			</filter-class>
			  </filter>
			  <filter-mapping>
			  	<filter-name>simpleCORSFilter</filter-name>
				    <url-pattern>/*</url-pattern>
				      </filter-mapping>

{% endhighlight %}

Redeploy both springwebapp and angularjswebapp to see results. It should display list of contacts when you click on the contact tab.



