---
title: "How to install gradle and use gradle for jenkins"
description: ""
category: 
tags: [jenkins, gradle]
---
{% include JB/setup %}

If you use jenkins and you have a project that uses gradle, how do you create a task that builds the project and deploy the artifact to tomcat?  
For this example we are going to use the netflix project [eureka](https://github.com/Netflix/eureka)

First install gradle on the slave/worker machine that is going to build the project.
{% highlight shell %} 
$ sudo add-apt-repository ppa:cwchien/gradle
$ sudo apt-get update
$ sudo apt-get install gradle
{% endhighlight %} 

To check that this works sudo su jenkins and type gradle -version (2.12 at time of this writing)  
Next install the jenkins plugin [deploy to container plugin](https://wiki.jenkins-ci.org/display/JENKINS/Deploy+Plugin)  
You might have to restart the jenkins instance for the plugin to take effect.

Inside the jenkins console Create a new task  

1. Select new item -> free-style
2. Enter project name, description
  * source code management -> git -> repository url (used my own because we have modify some settings in eureka, specify branch)
  * add build steps -> execute shell -> execute gradle script -> use gradle wrapper
  * switches -x test
  * tasks clean build 
3. post-build actions
  * war files eureka-server/build/libs/*.war
  * context path eureka
  * manager username
  * manager password
  * url http://tomcat.home.tw:8080/

TODO blog post on gradle deploy to tomcat using gradle cargo plugin.
