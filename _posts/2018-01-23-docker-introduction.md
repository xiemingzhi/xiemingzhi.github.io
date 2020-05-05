---
title: "Docker Introduction"
description: "Docker Java Spring Boot Example"
category: 
tags: [docker, java, spring boot]
---
{% include JB/setup %}

Familarize yourself with docker taken from: https://runnable.com/docker/java/dockerize-your-java-application  
The tutorial takes you from the base image to building a container that has java, tomcat with war deployment.  
The first example builds an image with java support and runs a java program: https://github.com/xiemingzhi/dockerjava  
The second example builds an image with openjdk baseimage and runs a spring boot application: https://github.com/xiemingzhi/dockerspringboot  
Using these examples it is possible to build an image that clones source code from a repository build then deploy it to any location.  

Things to consider for development/production environment:
1. build inside docker container or build on docker host then copy to container then start the container?
2. build on jenkins copy war to docker host then with Dockerfile copy to container then copy to webapps then start the container?
3. use docker container jenkins master -> slave, slave checkout build start container reference: https://www.thepolyglotdeveloper.com/2017/04/continuous-deployment-of-web-application-containers-with-jenkins-and-docker/

Next I will do a microservice example with docker containers, stay tuned!

