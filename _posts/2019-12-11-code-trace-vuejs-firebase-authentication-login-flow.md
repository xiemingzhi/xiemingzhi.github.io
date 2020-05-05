---
title: "Code trace vuejs firebase authentication login flow"
description: ""
category: 
tags: [firebase,vuejs]
---
{% include JB/setup %}

Using example from [https://github.com/kefranabg/bento-starter]()

Starting point NavBar.vue user clicks on Login, router sends to Login.vue.  
Login.vue 
```
    <div
      v-show="user !== undefined && !user && networkOnLine"
      data-test="login-btn"
      class="login-btn"
      @click="login"
    >
      Login with google
    </div>
...
    async login() {
      const provider = new firebase.auth.GoogleAuthProvider()
	  ...
        isDekstop()
          ? await firebase.auth().signInWithPopup(provider)
          : await firebase.auth().signInWithRedirect(provider)
    }
```
This uses the [Firebase API](https://firebase.google.com/docs/auth/web/google-signin).  
  
src\firebase\authentication.js 
```
import store from '@/store'

firebase.auth().onAuthStateChanged(firebaseUser => {
  const actionToDispatch = isNil(firebaseUser) ? 'logout' : 'login'
  store.dispatch(`authentication/${actionToDispatch}`, firebaseUser)
})
```
Vuex Store Callback is registered with Firebase API.  
  
src\store\authentication\authentication.actions.js  
```
  /**
   * Callback fired when user login
   */
  login: async ({ commit, dispatch }, firebaseAuthUser) => {
...
    commit('setUser', user)
    dispatch('products/getUserProducts', null, { root: true })
  },
```
This will retrieve the users products after they have successfully login.  
  
Login.vue also has a watch on user that will execute once vue detects it has been modified.
```
  watch: {
    user: {
      handler(user) {
        if (!isNil(user)) {
          const redirectUrl = isNil(this.$route.query.redirectUrl)
            ? '/products'
            : this.$route.query.redirectUrl
          this.$router.push(redirectUrl)
        }
      },
      immediate: true
    }
  },
```
User will be redirected to the `/products` page.

