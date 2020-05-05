---
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
The initial commit of the code does not contain a `.gitlab-ci.yml` file so the CI/CD build will fail. 

## Setup unit testing (test:unit)
Usually unit testing is used to test whether functions or single pieces work as intended. Always make sure the tests work in your local dev environment

See [vue unit testing guide](https://vue-test-utils.vuejs.org/guides/getting-started.html) for how to write your first unit tests for vue.

Create a `.gitlab-ci.yml` with the following contents:
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

## Setup integration testing (test:e2e)  
Integration testing are more complicated because they involve testing the UI(i.e lauching a browser headless), testing whether certain buttons appear or not and work from start to finish(i.e mocking some external apis). For this example we'll be using [cypress](https://www.cypress.io/). Follow the [install and test guide](https://docs.cypress.io/guides/getting-started/installing-cypress.html#System-requirements) for your first integration test.

Example `.gitlab-ci.yml` for integration testing:
```
image: cypress/browsers:node10.2.1-chrome74

before_script:
  - node --version
  - npm --version
  - npm clean-install
  - npx cypress cache path
  - npx cypress cache list
  - npx cypress verify
  - npx cypress info
  
# to cache both npm modules and Cypress binary we use environment variables
# to point at the folders we can list as paths in "cache" job settings
variables:
  npm_config_cache: "$CI_PROJECT_DIR/.npm"
  CYPRESS_CACHE_FOLDER: "$CI_PROJECT_DIR/cache/Cypress"
  
cache:
  paths:
    - node_modules/
    - cache/Cypress
    - .npm

test_e2e:
  script:
    - npm run test:e2e:headless --spec tests/e2e/specs/test.js
```
For this example we are using the docker image provided by cypress which contains a chrome browser preinstalled. This saves us from calling some browser install scripts and a faster test execution.

The `variables` section is really important because it determines where cypress will be cached, so when the test is run again it will be restored and the executable can be run.

## Troubleshooting
Use the gitlab console to view the errors from the builds(Project -> CI/CD -> Jobs)

```
node ./scripts/install.js || node-gyp rebuild
http 404 https://github.com/MayhemYDG/iltorb/releases/download/v2.4.3/iltorb-v2.4.3-node-v83-linux-x64.tar.gz
```
if you get node-gyp not found or unable to install node-gyp try different docker node images.

node-gyp is a cross-platform command-line tool written in Node.js for compiling native addon modules for Node.js. It contains a fork of the gyp project that was previously used by the Chromium team, extended to support the development of Node.js native addons.

There is alot of trial and error with gitlab ci, so I suggest making sure the tests work in your local dev environment. Create a test project in gitlab commit code to test project use test project to change .gitlab-ci.yml

------------------------------------------------------------------
Problem:
```
 Error: Failed to launch chrome!
 /builds/vue-cli-app/node_modules/puppeteer/.local-chromium/linux-650583/chrome-linux/chrome: error while loading shared libraries: libX11-xcb.so.1: cannot open shared object file: No such file or directory
 TROUBLESHOOTING: https://github.com/GoogleChrome/puppeteer/blob/master/docs/troubleshooting.md
     at onClose (/builds/vue-cli-app/node_modules/puppeteer/lib/Launcher.js:342:14)
     at Interface.<anonymous> (/builds/vue-cli-app/node_modules/puppeteer/lib/Launcher.js:331:50)
     at Interface.emit (events.js:327:22)
     at Interface.close (readline.js:424:8)
     at Socket.onend (readline.js:202:10)
     at Socket.emit (events.js:327:22)
     at endReadableNT (_stream_readable.js:1218:12)
     at processTicksAndRejections (internal/process/task_queues.js:84:21)
 [Prerenderer - PuppeteerRenderer] Unable to start Puppeteer
```
Solution:  
This error occurs because the docker image does not contain the necessary libraries to run chrome, make sure you use `image: cypress/browsers:node10.2.1-chrome74`

---------------------------------------------------------------------
Problem:
```
The cypress npm package is installed, but the Cypress binary is missing.
 We expected the binary to be installed here: /root/.cache/Cypress/3.8.3/Cypress/Cypress
 Reasons it may be missing:
 - You're caching 'node_modules' but are not caching this path: /root/.cache/Cypress
 - You ran 'npm install' at an earlier build step but did not persist: /root/.cache/Cypress
 Properly caching the binary will fix this error and avoid downloading and unzipping Cypress.
```
Solution:  
This problem occurs because you are caching node_modules use variables to specify cache folder 

