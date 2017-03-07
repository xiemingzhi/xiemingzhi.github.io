---
layout: post
category : lessons
tagline: "AngularJS Make Cross Site Requests"
tags : [intro, beginner, angularjs, tutorial]
date: 2017-02-14 16:16:01 +0800
---
{% include JB/setup %}

# AngularJS Sample Program to make cross site requests

1. Make sure java webapp providing service has enabled cors
modify web.xml
<filter>
  <filter-name>CorsFilter</filter-name>
  <filter-class>org.apache.catalina.filters.CorsFilter</filter-class>
</filter>

<filter-mapping>
  <filter-name>CorsFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>

2. Test OPTIONS request has been enabled - use postman to test
Request URL: http://localhost:8000/userman/services/api/users
Method: OPTIONS
Status: 200 OK


This is an example of a draft. Read more here: [http://jekyllrb.com/docs/drafts/](http://jekyllrb.com/docs/drafts/)
