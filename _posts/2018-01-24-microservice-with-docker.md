---
title: "Microservice with Docker"
description: ""
category: 
tags: [microservice docker]
---
{% include JB/setup %}

This example uses docker-compose to create three containers.  
It is a microservice-example with these functions: db, restful, ui.  
**db:** docker-mysql-example startup mysql create database create user assign priviledge;  
**restful:** docker-restful-example uses spring mvc to create a restful service and deploy on tomcat;  
**ui:** springangular+zuul+docker-angularjs-example a spring boot application with zuul support and an angularjs 1.5 frontend;  
This demonstrates the use of an edge point service api gateway to handle proxy requests to other services using netflix zuul  
Everything is contained in one github repos: https://github.com/xiemingzhi/microserviceproject  
The ui container is a problem because you have to build the angularjs project first then copy the dist contents to the spring boot project static directory then build the project, after you have the jar then you can build the container.  

TODO: one docker-compose.yml 4 separate directories cors example frontend separate container

