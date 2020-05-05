---
title: "How to enable OS Login in vm instance"
description: ""
category: "DevOP"
tags: [gcp,compute-engine]
---
{% include JB/setup %}

This is for google cloud computing platform.  
Goto compute engine and create [ubuntu](https://console.cloud.google.com/marketplace/details/ubuntu-os-cloud/ubuntu-bionic) bionic(18.04) instance.  
To enable [OS Login](https://cloud.google.com/compute/docs/instances/managing-instance-access) for all projects goto metadata add key:value "enable-oslogin:TRUE"  
Enabling OS Login will cause all your login names to be username_gmail_com  

To add your public key install [gcloud sdk](https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe).  
If the installer does not work download [sdk](https://console.cloud.google.com/storage/browser/cloud-sdk-release) zip.  
Requires python 3.5.  
Unzip and run install.bat reply [Y] to add PATH.
The sdk requires google login open terminal:
```
gcloud init 
or 
gcloud init --console-only
```
If the first one doesn't work use the second one, command will provide url copy into chrome enter and copy the verfication code back into the terminal.

If success gcloud will allow you to login to vm instance:
```
gcloud compute instances list 
gcloud compute ssh ubuntu-bionic-1
```
where `ubuntu-bionic-1` is your vm instance name.

Use gcloud to add your public key.
[Public key format](https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys#sshkeyformat): ssh-rsa [KEY_VALUE] [USERNAME]
```
gcloud compute os-login ssh-keys add --key-file c:\users\username\.ssh\id_rsa.pub
gcloud compute os-login ssh-keys list 
```
Use third party to login putty etc:
```
ssh username_gmail_com@console.instance.ip
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 5.0.0-1025-gcp x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Dec 23 01:24:41 UTC 2019

  System load:  0.0               Processes:           92
  Usage of /:   11.9% of 9.52GB   Users logged in:     0
  Memory usage: 6%                IP address for ens4: xx.xx.xx.xx
  Swap usage:   0%


0 packages can be updated.
0 updates are security updates.
```

