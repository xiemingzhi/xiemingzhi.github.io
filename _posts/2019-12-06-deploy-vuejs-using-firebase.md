---
title: "Deploy vuejs using firebase"
description: ""
category: "DevOP"
tags: [gcp,vuejs]
---
{% include JB/setup %}

Install node9.3.0+ and npm@5.5.0+ before executing below commands.  

Checkout sample vuejs project: [https://github.com/kefranabg/bento-starter.git]()

Signup for google firebase account: [https://console.firebase.google.com/]()

Edit src\firebase\init.js for example:
```
const firebaseConfig = {
  apiKey: 'AIzaSyC-5Lc9hvJYvqI__xxxxxxxx',
  authDomain: 'vue-project-895891584111.firebaseapp.com',
  databaseURL: 'https://vue-project-895891584111.firebaseio.com',
  projectId: 'vue-project-895891584111',
  storageBucket: 'vue-project-895891584111.appspot.com',
  messagingSenderId: '895891584111',
  appId: '1:895891584111:web:2cbd2ea34bf0d32dcce555'
}
```

Create .firebaserc file for example:
```
{
  "projects": {
    "default": "vue-project-895891584111"
  }
}
```

Modify the settings for your own project according firebase console.

```
npm run setup
npm i -g npx
# login with google account
npx firebase login
npx firebase use --add
# commit .firebaserc & init.js
npm run build
# run and test application locally
npm run serve
# if application is okay then deploy to google
npx firebase deploy
```
Open browser: [https://vue-project-895891584111.firebaseapp.com/]()  
Change url to match yours.

# Bonus

If you want to setup an subdomain for your application.   For example map vue-project-895891584111.firebaseapp.com to bento.example.com

To do the following your must have access to your domain's dns settings.

Goto firebase console, select application just created, click 'Connect Domain', enter subdomain name: bento.example.com, select verification, it will display a 'TXT Record' which you will have to enter in your domain provider's dns settings.

Example is for namecheap:  
Enter under 'Host records'
```
TXT Record	@ google-site-verification=XXXguZlE9o31VA0pdVDl2ymA39H6KSVFs2CTsh6ixxx
```

Check using: 
[https://dnschecker.org/#TXT/example.com]()
  
| Type |  Domain Name   |  TTL   |                                Record                                |
|------|----------------|--------|----------------------------------------------------------------------|
| TXT  | example.com | 60 sec | google-site-verification=XXXguZlE9o31VA0pdVDl2ymA39H6KSVFs2CTsh6ixxx |
| TXT  | example.com | 30 min | v=spf1 include:spf.efwd.registrar-servers.com ~all                   |
  
Note: namecheap will have another '@' record for mail, this is okay just make sure the google one has propagated to all DNS servers.

Go back firebase console click on 'Verify', if success then it will provide 'A Records' which you will have to enter in your domain provider's dns settings:  
```
A Record bento 151.101.1.195
A Record bento 151.101.65.195
```

Google will apply for Let's Encrypt certificate for your subdomain.  
Wait for changes to propagate through system, check using:
[https://mxtoolbox.com/SuperTool.aspx?action=https%3abento.example.com&run=toolpage#]()

