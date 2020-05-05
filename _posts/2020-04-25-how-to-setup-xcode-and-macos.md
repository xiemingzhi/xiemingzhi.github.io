---
title: "How to setup xcode and macos"
description: ""
category: 
tags: [apple,ios,swift]
---
{% include JB/setup %}

This is a guide to help people choose which versions of xcode to use with macos. Different versions of xcode work different macos versions, so it is important to install the corrent versions of macos before starting, or you will end up like me downloading all the versions of macos installing and then finding out it doesn't work with xcode.  

Wikipedia does have a page listing the [xcode](https://en.wikipedia.org/wiki/Xcode) versions with minimal macos versions

macos versions with names:
```
OS X 10.8 Mountain Lion
OS X 10.9 Mavericks
OS X 10.10 Yosemite
OS X 10.11 El Capitan
macOS 10.12 Sierra
macOS 10.13 High Sierra
macOS 10.14 Mojave
macOS 10.15 Catalina
```

The wiki lists the xode versions from 11.0 - 11.3.1 requirement min macos version 10.14.4 Mojave, but if you install 10.14 Mojave now(2020), you can't upgrade to 10.14.4, you can only upgrade to 10.15 Catalina. So install 10.15 Catalina and download xcode 11.4 for full compatibility.

The latest xode uses swift so if you have old projects that use objective-c they might not work.

Project settings 

[//]: #![1]({{site.url}}/assets/xcode-project-settings.png)
<img src="{{site.url}}/assets/xcode-project-settings.png" width="800">

AppDelegate change

[//]: #![1]({{site.url}}/assets/xcode-appdelegate-change.png)
<img src="{{site.url}}/assets/xcode-appdelegate-change.png" width="800">


