---
title: "Fixing cacti"
description: ""
category: "OS"
tags: [systemintegration,cacti]
---
{% include JB/setup %}

If you use ubuntu 18.04 and use nginx as your webserver and want to install cacti there are a few things to watch out for.  
cacti is php based but when you install php via apt it will install apache2 as well.  
If you remove the apache2 package then there will be some un-needed dependencies. If you autoremove those dependencies then your cacti will not work.  

# How to fix 

cacti homepage error 
```
FATAL: Connection to Cacti database failed. Please ensure:

the PHP MySQL module is installed and enabled.
the database is running.
the credentials in config.php are valid.
```
testing 
```
mysql -u cactiuser -pcactipasswd -h localhost -P 3306 -D cactidb
```
if mysql works then test using php
``` 
php -r "new PDO('mysql:host=localhost;port=3306;dbname=cactidb', 'cactiuser', 'cactipasswd');"
PHP Fatal error:  Uncaught PDOException: could not find driver in Command line code:1
Stack trace:
#0 Command line code(1): PDO->__construct('mysql:host=loca...', 'cactiuser', 'cactipasswd')
#1 {main}
  thrown in Command line code on line 1
```  
sudo vi /etc/php/7.2/cli/php.ini  
uncomment extension=php_pdo_mysql.dll  
```
$ php -r "new PDO('mysql:host=localhost;port=3306;dbname=cactidb', 'cactiuser', 'cactipasswd');"
PHP Warning:  PHP Startup: Unable to load dynamic library 'pdo_mysql' (tried: /usr/lib/php/20170718/pdo_mysql (/usr/lib/php/20170718/pdo_mysql: cannot open shared object file: No such file or directory), /usr/lib/php/20170718/pdo_mysql.so (/usr/lib/php/20170718/pdo_mysql.so: cannot open shared object file: No such file or directory)) in Unknown on line 0
```
sudo apt install php-mysql
```
PHP Warning:  PHP Startup: Unable to load dynamic library 'pdo_mysql' (tried: /usr/lib/php/20170718/pdo_mysql (/usr/lib/php/20170718/pdo_mysql: cannot open shared object file: No such file or directory), /usr/lib/php/20170718/pdo_mysql.so (/usr/lib/php/20170718/pdo_mysql.so: undefined symbol: mysqlnd_allocator)) in Unknown on line 0
```
sudo apt install php-mysqlnd  
sudo apt install php  
sudo apt remove --purge php7.2-mysql  
sudo apt remove --purge php-mysqlnd  
sudo apt remove --purge php7.2-cli  
sudo apt remove --purge php7.2  
sudo apt install php-mysql  
sudo apt install php-cli  
sudo apt install php  
stop/disable apache2  
use systemctl commands to prevent a service from automatically starting at boot.
```
sudo systemctl disable apache2
```
502 bad gateway
``` 
2019/11/29 15:00:13 [crit] 8894#8894: *697 connect() to unix:/var/run/php/php7.2-fpm.sock failed (2: No such file or directory) whileconnecting to upstream, client: 192.168.12.28, server: _, request: "GET /cacti/index.php HTTP/1.1", upstream: "fastcgi://unix:/var/run/php/php7.2-fpm.sock:", host: "192.168.1.2"
```
sudo apt remove --purge php7.2-fpm  
sudo apt install php-fpm  
```
2019/11/29 15:04:01 [error] 8894#8894: *766 FastCGI sent in stderr: "PHP message: PHP Fatal error:  Uncaught Error: Call to undefinedfunction mb_strlen() in /usr/share/cacti/lib/rrd.php:1593
Stack trace:
#0 /usr/share/cacti/graph_json.php(158): rrdtool_function_graph(12, '0', Array)
```
mbstring not enabled by default in php  
reinstall mbstring  
sudo apt install php-mbstring  
reinstall rrdtool  
sudo apt install rrdtool  
snmp tools missing  
sudo apt install snmp  

