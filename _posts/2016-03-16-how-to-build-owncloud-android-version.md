---
layout: post
title: "How to build owncloud android version"
description: ""
category: 
tags: []
---
{% include JB/setup %}

To build owncloud android you need a JDK and the [Android SDK](http://developer.android.com/sdk/index.html) 

Start the SDK Manager and make sure to install the API 19 and API 23 platform and the SDK Build Tools.

Add the ANDROID_SDK_HOME/tools to your PATH.

Git clone the [ownCloud android](http://github.com/owncloud/android) project.

cd android && execute setup_env.sh gradle

gradlew.sh clean build

apk is in the builds/output/apk directory


