---
layout: post
title: "AngularJS project to call RESTful service"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Trying to modify an angularjs dashboard app [angular-material-dashboard](https://github.com/flatlogic/angular-material-dashboard) to use the RESTful ContactService.

This is a node.js program so requirements are:

Node.js / npm

 $ cd project-directory
 $ npm install

Run web-server:

 $ gulp serve

## To Deploy  
  
 $ gulp build

This will create a dist/ directory with all the deliverables. Copy/FTP to your webserver.

Create src/app/controllers/ContactController.js

{% highlight javascript %}
function ContactController($scope, $http) {
    $http.get('https://your-project-id.appspot.com/contact/jsonlist.json').
        success(function(data) {
            $scope.contactData = data;
        });
  }
{% endhighlight %}  

Problem occurs when trying to run program:

{% highlight javascript %}
TypeError: Cannot read property 'get' of undefined
    at new ContactController 
{% endhighlight %}  

Create src/app/components/services/ContactService.js

{% highlight javascript %}
function contactService($q, $http){

$http.get('https://your-project-id.appspot.com/contact/jsonlist.json').
        success(function(data) {
            contactData = data;
        });
{% endhighlight %}  
		
Problem occurs when trying to run program:

{% highlight javascript %}
TypeError: Cannot read property 'get' of undefined
    at new contactService 
{% endhighlight %}  

Don't know why the $http variable is not being injected. 

Going to try to modify another angularjs project [ang-starter](https://github.com/mriverodorta/ang-starter/).

