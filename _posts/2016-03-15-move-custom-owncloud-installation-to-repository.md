---
title: "Move custom owncloud installation to repository"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Been using custom installation of [ownCloud](https://owncloud.org) since it first came out. But now see that they have Ubuntu repos installation option so moving custom installation to repos.

Steps to take when moving:

1. Create backup of ownCloud database
  * mysqldump -uUsername -pPassword -h hostname owncloudDB > /mnt/StorageLocation/ownCloud/ownclouddb-date.sql
2. Create backup of your files on ownCloud
  * Use the pc version of ownCloud to copy the files to another directory
3. Delete custom version of ownCloud on server
  * rm -rf /var/www/owncloud
4. Install owncloud
  * sudo apt-get install owncloud
5. Config owncloud
  * Use browser -> http://hostname/owncloud -> press storage & database options
6. Fill in the storage options (apache2 must have write permissions) & Fill in the database access details.

You might want to regularly backup your ownclouddb, here is a cronjob script to help:

5 8 1 * * mysqldump -uUsername -pPassword -h hostname ownclouddb > /mnt/StorageLocation/ownclouddb-$(date "+%Y-%m-%d-%H%M%S").sql

TODO: Future post on how to build android version of ownCloud.




