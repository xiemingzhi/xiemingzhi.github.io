---
title: "Setup multiple sites for your project using firebase"
description: ""
category: "DevOP"
tags: [gcp,firebase]
---
{% include JB/setup %}

## Why you need multiple sites 

You need to separate sites for production, staging, development so that you don't mess up client's data and you can do testing.

[multisites](https://firebase.google.com/docs/hosting/multisites)
```
 To mirror your workflow environments (for example, Dev, Q1, Q2, Prod), we recommend that you create a separate Firebase project for each environment rather than creating multiple sites in a single Firebase project. 
 Generally, you don't want to use production-environment Firebase resources (like customer data in a Realtime Database) in a development environment.
```
 
## Things to watch out for 

![Firebase console project settings]({{site.url}}/assets/Project-Settings-Firebaseconsole.png)  
- [regions](https://cloud.google.com/compute/docs/regions-zones/?hl=en#locations) 
  - [Function Locations](https://cloud.google.com/functions/docs/locations) Where you deploy your site can affect the speed and cost.
  
## Things to do 

- Install firebase-tools (nodejs, npm)
- Check in the right environment `firebase projects:list` Set project `firebase use project-name`
- [export users](https://firebase.google.com/docs/cli/auth) `firebase auth:export users.json`
- change project 
- [import users](https://github.com/firebase/firebase-tools/issues/253) `firebase auth:import users.json --hash-algo=scrypt --hash-key=xxx --salt-separator=xxx --rounds=8 --mem-cost=14` 
  - [project authentication hash settings]({{site.url}}/assets/project-authentication-hash.png) example hash-key, salt-separator settings 
- Install anaconda setup [python env](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html#managing-python) 
- Install [gcloud sdk](https://cloud.google.com/sdk/install) 
- enable [Cloud Storage](https://cloud.google.com/storage)
- export data 
  ```
  gcloud config set project project-id
  gcloud firestore export gs://project-id.appspot.com/all-collections-20200218
  gcloud firestore operations list 
  ```
- import data 
  ```
  gcloud config set project project-id2 
  gsutil cp -r gs://project-id.appspot.com/all-collections-20200218 gs://project-id2.appspot.com 
  gcloud firestore import gs://project-id2.appspot.com/all-collections-20200218
  ```
  
  