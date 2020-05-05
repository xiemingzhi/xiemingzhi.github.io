---
title: Openshift project
---


In order to support my android project I needed a backend. Chose openshift because it is free, have login capabilities and can run jboss. First go through the [openshift registration process](https://openshift.redhat.com) to register for an account. It is free and you get 3 free gears, gears are like instances.

Create a ssh public key and upload to the openshift console. This will be used to access your instance and for pulling and pushing code.

Start up a JBoss Application Server 7 instance and attach a MySQL Cartridge. Choose a meaningful name for your instance. Note at the end of the start up process it provides login details for your server and the database access rights. For example:

{% highlight git %}
git clone ssh://your-instance-id@your-server-domain.rhcloud.com/~/git/jbossas.git/
cd jbossas/
{% endhighlight %}

Use git to clone the project and start to add code to it. It is a maven project so as long as you are using public dependencies then it should compile on openshift as well. You push code to openshift then it compiles, packages then deploys the war to your jboss application server instance.

When you added the database cartridge it will provide credential information like the following:
<pre>
MySQL 5.5 database added.  Please make note of these credentials:

       Root User: adminusername
   Root Password: adminpassword
   Database Name: your-server-name

Connection URL: mysql://$OPENSHIFT_MYSQL_DB_HOST:$OPENSHIFT_MYSQL_DB_PORT/
</pre>

To know what the $OPENSHIFT_MYSQL_DB_HOST:$OPENSHIFT_MYSQL_DB_PORT is use ssh login to your instance and type 'env' to get at the variables.

{% highlight git %}
ssh your-instance-id@your-server-domain.rhcloud.com
env
...
OPENSHIFT_MYSQL_DB_PORT=3306
...
OPENSHIFT_MYSQL_DB_HOST=your-instance-ip
...
{% endhighlight %}

It will use the public key that you've uploaded to provide access.

The server application logs are located at '==> app-root/logs/jbossas.log <=='. You can also install 'rhc' a commandline utility to manage your instance. It uses ruby so remember to install latest version of 1.9.3.

