---
title: "Implementing payments with vuejs and firebase"
description: "Payments google pay stripe vuejs firebase"
category: 
tags: [payments,vuejs]
---
{% include JB/setup %}

# Payments 

The [Payments Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API) is a standard that provides a consistent interface to capture user payments information.   
This only captures the user's payment information we still have to pass this information to the [payment gateway](https://developers.google.com/pay/api/web/guides/paymentrequest/tutorial).  
Most important concept using this api still requies a PSP([Payments Service Provider](https://developers.google.com/web/fundamentals/payments/basics/how-payment-ecosystem-works)): Stripe BrainTree [etc](https://developers.google.com/pay/api#participating-processors) 

# Stripe 

Firebase provides an example for integration with [stripe](https://firebase.google.com/docs/use-cases/payments).  
It is also written using [vue](https://github.com/firebase/functions-samples/blob/master/stripe/public/index.html).  
Sign up for an account use the test token.  
Don't use the npm stripe that is for server usage.  
Have to include js in html:
```
<script src="https://js.stripe.com/v3/"></script>
``` 
To use in vue use `mounted` to check/init Stripe object 
```
  mounted: function() {
    this.setupStripe()
  },
  methods: {
    setupStripe() {
      if (window.Stripe === undefined) {
        alert('Stripe V3 library not loaded!')
      } else {
        console.log('Stripe library loaded')
        const stripe = window.Stripe(
          'pk_test_GZYfG6M52r52tEHYj4G5mUkz'
        )
        this.stripe = stripe
        const elements = stripe.elements()
        const style = {
          base: {
            color: '#32325d',
            fontFamily: '"Helvetica Neue", Helvetica, sans-serif',
            fontSmoothing: 'antialiased',
            fontSize: '16px',
            '::placeholder': {
              color: '#aab7c4'
            }
          },
          invalid: {
            color: '#fa755a',
            iconColor: '#fa755a'
          }
        }
        this.card = elements.create('card', { style: style })
        this.card.mount('#card-element')
      }
```
Have to place div elements from stripe inside form to render object
```
    <b-form class="card-form">
      <label for="card-element">
        信用卡繳款
      </label>
      <div id="card-element"></div>
      <br />
      <div class="text-center">
        <b-button class="button-pay" @click="doPayment()">付款</b-button>
      </div>
    </b-form>
```

For production deployment there is https requirement or else you will get this error:
```
You may test your Stripe.js integration over HTTP. However, live Stripe.js integrations must use HTTPS.
```

Update on the form element.  
If you want to use the stripe integration example from firebase don't put the credit card details capture inside the form.  
The form element click event will cause the page to be submitted causing a refresh losing the captured data.  
Instead use: 
```
    <div>
      <h4>新增信用卡</h4>
      <div>
        <label> 卡號 <input v-model="newCreditCard.number" /> </label>
      </div>
      <div>
        <label> 安全碼 <input v-model="newCreditCard.cvc" /> </label>
      </div>
      <div>
        <label>
          到期日
          <input v-model="newCreditCard.exp_month" size="2" /> /
          <input v-model="newCreditCard.exp_year" size="4" />
        </label>
      </div>
      <div>
        <label> 郵遞區號 <input v-model="newCreditCard.address_zip" /> </label>
      </div>
      <div>
        <button @click="submitNewCreditCard">增加</button>
        {{ newCreditCard.error }}
      </div>
    </div>
```


