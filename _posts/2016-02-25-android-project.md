---
layout: post
title: Android studio - Android project
---

{{ page.title }}
================

Starting an android project to call some webservices.<br>
Using android studio 2.0. Android studio will install the android sdk for you but if you have already an installation change it so that it points to your location.<br>
Setup your android sdk, during the installation of android studio it will install alot android versions which will bloat your installation. If you don't like this just startup sdk manager and tick the ones you don't want (leave the one that you will be developing for) and press the Delete packages button.
![Blogger export screenshot]({{ site.url }}/assets/sdk-setup.png)
<br>
Setup your avd (android virtual device), startup avd manager, select Devices, select the device that you will developing for or choose the one closest matching then press Create AVD button.<br>
In the list of AVDs select the one that you just created then Edit it:
![Blogger export screenshot]({{ site.url }}/assets/avd-setup.png)
<br>
This is a windows setup with an intel pc so make sure you have installed the [intel hardware accelerate execution manager](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/)
<br>

