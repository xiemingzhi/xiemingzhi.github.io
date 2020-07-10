---
title: Tracing PWA new content refresh event
date: 2020-07-10T20:54:40+08:00
---

I was using this wrapper around PWA(Progress Web Apps) APIs [register-service-worker](https://github.com/yyx990803/register-service-worker) and was wondering how does it detect when there is a new build and the page must refresh?
So this is how you use the wrapper:

```
  register('/service-worker.js', {
    ready() {
      // eslint-disable-next-line
      console.log('Service worker is active.')
    },
...
    updated(reg) {
      store.commit(`app/setSWRegistrationForNewContent`, reg)
      // eslint-disable-next-line
      console.log('New content is available; please refresh.')
    },
...
  }
```

Turns out it is following the Service Worker API defined in the documents:
* [https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerContainer/register]()
* [https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerRegistration/onupdatefound]()

The `register` method returns a `ServiceWorkerRegistration` object:
```
serviceWorkerContainer.register(scriptURL, options)
  .then(function(serviceWorkerRegistration) { ... });
```
The `ServiceWorkerRegistration` contains a `onupdatefound` event handler that is called whenever an event of type updatefound is fired; So whenever it detects a new service worker is being installed it will call this method. 
```
serviceWorkerRegistration.onupdatefound = function() { ... };
```
In this wrapper we are passing the updated(reg) callback function to this method, which will be called when ever it detects a new version of our app has been deployed.
