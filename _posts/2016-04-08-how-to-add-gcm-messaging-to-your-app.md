---
title: "How to add gcm messaging to your app"
description: ""
category: 
tags: [android]
---
{% include JB/setup %}

If you tried the gcm playground and it does not work then try gcm quickstart tutorial.

{% highlight git %}
git clone https://github.com/googlesamples/google-services.git
{% endhighlight %}

You need to have a google developers account and created an sample empty project using the developers console.

Use the following link to get your configuration file [google-services.json](
https://developers.google.com/mobile/add?platform=android&cntapi=gcm&cntapp=Default%20Demo%20App&cntpkg=gcm.play.android.samples.com.gcmquickstart&cnturl=https:%2F%2Fdevelopers.google.com%2Fcloud-messaging%2Fandroid%2Fstart%3Fconfigured%3Dtrue&cntlbl=Continue%20with%20Try%20Cloud%20Messaging)

For the package name use: gcm.play.android.samples.com.gcmquickstart

You need to store the Server API Key and Sender ID in a safe location.

Save the google-services.json file into the google-services/android/gcm directory.  

{% highlight shell %}
cd google-services/android/gcm
./gradlew assemble
{% endhighlight %}

Use gradlew assemble to build app  
If you use gradle and experience build errors then you might have an older version of gradle installed use instead the gradlew script to get the appropriate version.  
gradlew might have problems locating the SDK location make sure to specify ANDROID_HOME  
Install the apk located at app/build/outputs/apk

Modify the GcmSender.java file with the API_KEY from previously, file located at gcmsender\src\main\java\gcm\play\android\samples\com\gcmsender
use gcmsender to send message
{% highlight shell %}
./gradlew run -Pmsg="<message>"
{"message_id":5323982615956046161}
{% endhighlight %}
Check your device/emulator for notification or logcat for confirmation of the receipt of the GCM message.

**Adding gcm messaging to your app**  

* Add google dependencies to build.gradle  
{% highlight shell %}
dependencies {
...
compile 'com.google.android.gms:play-services-gcm:8.4.0'
...
}
apply plugin: 'com.google.gms.google-services'
{% endhighlight %}  

* Add services to AndroidManifest.xml  
{% highlight xml %}
    <application
	...
        <receiver
            android:name="com.google.android.gms.gcm.GcmReceiver"
            android:exported="true"
            android:permission="com.google.android.c2dm.permission.SEND" >
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="com.mycompany.android.androidproject" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.mycompany.android.androidproject.MyGcmListenerService"
            android:exported="false" >
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            </intent-filter>
        </service>
        <service
            android:name="com.mycompany.android.androidproject.MyInstanceIDListenerService"
            android:exported="false">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID"/>
            </intent-filter>
        </service>
        <service
            android:name="com.mycompany.android.androidproject.RegistrationIntentService"
            android:exported="false">
        </service>
	...
	</application>
{% endhighlight %}
* MainActivity initialize broadcast receiver, register broadcast receiver, check google play services, start register service  
* Register Intent Service Register with gcm, obtain instanceid, Subscribe to topic  
* GCM Listener Service implement onMessageReceived get message from Bundle, show notification  
