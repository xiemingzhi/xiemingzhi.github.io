---
layout: post
title: "Setup gitlab ci unit and integration"
description: ""
category: "DevOP"
tags: [gitlab,ci]
---
{% include JB/setup %}

This is a guide on how to setup a vuejs, with vue-cli-service, project to work with gitlab ci.

[Gitlab](https://gitlab.com/) is a source control management system together with continuous integration capabilities. Gitlab is open source and does have a community edition. This means you can submit your code to gitlab and it will build, test and deploy to your environment.

Requirements:
- node: > 10.0
- vuejs: > 2.6
- gitlab: > 12.7

This guide assumes you have already setup/installed a version of gitlab or have run a version running from the cloud.

## Setup gitlab 
For this version install [gitlab using docker](https://docs.gitlab.com/omnibus/docker/#install-gitlab-using-docker-compose). Also start [gitlab runner](https://docs.gitlab.com/runner/install/docker.html).
```
docker run -d --name gitlab-runner --restart always \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
``` 
Note: gitlab-runner requires alot of resources make sure your server has the [requirements](https://docs.gitlab.com/ee/install/requirements.html#gitlab-runner).
 
## Create vue project

```
vue create hello-world
```

Create a project in gitlab and check the Project -> Settings -> CI/CD. Get the register_token, issue the following command to register the gitlab runner for the project:
```
docker exec -it gitlab-runner \
  gitlab-runner register \
    --non-interactive \
    --registration-token REGISTER_TOKEN \
    --locked=false \
    --description docker-node \
    --url https://your.domain.com/ \
    --executor docker \
    --docker-image node:10.20.1-jessie
```
Replace REGISTER_TOKEN with the token you get from gitlab web console. Replace your.domain.com with the url you use to access gitlab. I am using the docker image node:10.20.1-jessie because the project is a vuejs/node project, feel free to change it to whatever suits you, see hub.docker.com.
 
Commit the code.  
The initial commit of the code does not contain a `.gitlab-ci.yml` file so the CI/CD build will fail. Create a `.gitlab-ci.yml` with the following contents:
```
image: node:10.20.1-jessie

cache:
  paths:
    - node_modules/

before_script:
  - node --version
  - npm --version
  - npm clean-install

test_unit:
  script:
    - npm run test:unit

```

## Setup unit testing (test:unit)
Usually unit testing is used to test whether functions or single pieces work as intended. Always make sure the tests work in your local dev environment

## Setup integration testing (test:e2e)  
Integration testing are more complicated because they involve testing the UI, whether certain buttons appear and work from start to finish.

## Troubleshooting
Use the gitlab console to view the errors from the builds(Project -> CI/CD -> Jobs)

```
node ./scripts/install.js || node-gyp rebuild
http 404 https://github.com/MayhemYDG/iltorb/releases/download/v2.4.3/iltorb-v2.4.3-node-v83-linux-x64.tar.gz
```
if you get node-gyp not found or unable to install node-gyp try different docker node images.

node-gyp is a cross-platform command-line tool written in Node.js for compiling native addon modules for Node.js. It contains a fork of the gyp project that was previously used by the Chromium team, extended to support the development of Node.js native addons.

There is alot of trial and error with gitlab ci, so I suggest making sure the tests work in your local dev environment. Create a test project in gitlab commit code to test project use test project to change .gitlab-ci.yml




