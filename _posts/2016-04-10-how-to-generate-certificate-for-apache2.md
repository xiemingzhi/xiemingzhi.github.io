---
title: "How to generate certificate for apache2"
description: ""
category: 
tags: [apache]
---
{% include JB/setup %}

If you need to setup ssl ('https') connections to your apache2 webserver then use the following steps.

{% highlight shell %} 
sudo mkdir /etc/apache2/ssl
sudo openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:New York
Locality Name (eg, city) []:New York City
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Your Company
Organizational Unit Name (eg, section) []:Department of Kittens
Common Name (e.g. server FQDN or YOUR name) []:your_domain.com
Email Address []:your_email@domain.com
{% endhighlight %} 

Modify the apache2 configuration files:
{% highlight shell %} 
sudo vi /etc/apache2/sites-enabled/default-ssl.conf
SSLCertificateFile /etc/apache2/ssl/apache.crt
SSLCertificateKeyFile /etc/apache2/ssl/apache.key
sudo service apache2 restart
{% endhighlight %} 

If your application is a web application and you want to access from a mobile device then goto website download cer and it save to device.  
Android has an option in the Settings to load the certificate from external storage.


