---
title: "How to export data from firestore"
description: ""
category: 
tags: [cloud-storage,firestore]
---
{% include JB/setup %}

# Introduction 

If you use Firestore then this guide will show you how to export data to Cloud Storage.  
After you have created a project in Firebase enable cloud storage, this will require a paid account/account with billing enabled.  
When creating the project make sure to choose the right region for the project because cloud storage creation is also tied to the same region.  

## Enable cloud storage 

[Default GCP resource location](https://firebase.google.com/docs/projects/locations?authuser=0)
```
Warning: Setting the location for one of the following services also sets the location for the others. After you set your project's default GCP resource location, you cannot change it.
```
Also set some restrictions on access [cloud storage security](https://firebase.google.com/docs/storage/security/start?authuser=0#sample-rules)

You can use the Firebase console to add some collections/documents to the Firestore.

To export data to cloud storage you have to install the [google cloud sdk](https://cloud.google.com/sdk/install) which requires python, for windows install [Anaconda](https://www.anaconda.com/distribution/)

open anaconda terminal use `gcloud config list` to see your project configurations, make sure you're working on the right project
```
[compute]
region = asia-east1
zone = asia-east1-b
[core]
account = xxx@gmail.com
disable_usage_reporting = True
project = pelagic-logic-666
Your active configuration is: [default]
```
To change project
```
gcloud config set project api-project-123456789012
```

### export all documents 
```
gcloud firestore export gs://api-project-123456789012.appspot.com
...
  operationState: PROCESSING
  outputUriPrefix: gs://api-project-123456789012.appspot.com/2019-12-30T02:26:58_99921
...
```

### import documents 
```
gcloud firestore import gs://api-project-123456789012.appspot.com/2019-12-30T02:26:58_99921/
...
  inputUriPrefix: gs://api-project-123456789012.appspot.com/2019-12-30T02:26:58_99921
  operationState: PROCESSING
...
```

### export collections 
```
gcloud firestore export gs://api-project-123456789012.appspot.com --collection-ids=providers
...
outputUriPrefix: gs://api-project-123456789012.appspot.com/2019-12-30T03:18:47_53425
```

### import collections 
Use the outputUriPrefix from export as import uri:
```
gcloud firestore import gs://api-project-123456789012.appspot.com/2019-12-30T03:18:47_53425
...
```

### export subcollections
This exports the collection `users` and its subcollections `orders`
```
gcloud firestore export gs://api-project-123456789012.appspot.com --collection-ids=users,orders
...
outputUriPrefix: gs://api-project-123456789012.appspot.com/2019-12-30T03:30:11_81009
```
 
### import subcollections 
Use the outputUriPrefix from export as import uri:
```
gcloud firestore import gs://api-project-123456789012.appspot.com/2019-12-30T03:30:11_81009 --collection-ids=orders
```

### list all operations 
All operations start in `PROCESSING` mode to see status:
```
gcloud firestore operations list 
---
done: true
metadata:
  '@type': type.googleapis.com/google.firestore.admin.v1.ExportDocumentsMetadata
  collectionIds:
  - users
  - orders
  endTime: '2019-12-30T03:30:20.200061Z'
  operationState: SUCCESSFUL
  outputUriPrefix: gs://api-project-123456789012.appspot.com/2019-12-30T03:30:11_81009
...  
```
Look for `SUCCESSFUL` message.

### Export cloud storage to local machine 

Use [gsutil](https://cloud.google.com/storage/docs/quickstart-gsutil) to copy to local machine  
gsutil is contained in cloud sdk 
if you installed the offline version then set environment variable
```
CLOUDSDK_PYTHON=C:\Users\username\Anaconda3\python.exe
```
open anaconda 
```
gsutil config 
```
export with folder name
```
gcloud firestore export gs://api-project-123456789012.appspot.com/all-collections
```
copy to local
```
gsutil cp -r gs://api-project-123456789012.appspot.com/all-collections d:\tmp\firestore
```

