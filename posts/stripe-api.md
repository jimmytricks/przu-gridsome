---
title: Stripe's API and Custom Payment Amount
date: 2019-03-04
description: Implementing a PHP and cURL approach for a custom payment amount
slug: stripe-api-custom-amount
blogpost: true
---


## Overview

In this article, I will outline how I used the Stripe.js and [Elements library](https://stripe.com/docs/stripe-js/elements/quickstart) to create a more customized payment checkout, and one that includes a user-entered amount. 

The source code can be found here [https://github.com/jimmytricks/stripe-custom-amount](https://github.com/jimmytricks/stripe-custom-amount)

## Working with Stripe's API

The Stripe API is a pleasure to work with, though one area I did find a little lacking in the documentation, as of writing, was accepting custom amounts. 

First things first, I installed the PHP library for Stripe with:

`composer require stripe/stripe-php`

Then included `require_once('vendor/autoload.php');` at the top of my paymentform.php, which I'll come to later. 

Here is how I setup my payment page:
```
// Include the stripe payment JS and my own processing JS file
<script src="https://js.stripe.com/v3/"></script>
<script src="/processpayment.js" defer></script>

<form action="/paymentform.php" method="post" id="payment-form">
 <!-- Usual form fields go here, name, email etc -->
    <div class="form-row">
        <div id="card-element">
        </div>
    </div>
    <div id="card-errors" role="alert">
    </div>
    <div class="form-row sub-row">
        <button>Submit Payment</button>
    </div>
</form>
```
Nothing too special here. The stripe library is loaded, along with my process payment JS file. The form includes the id of card-element, which will become the section for entering payment card details, while the card-errors does what you would expect and shows any errors. 

Here is the process payment JS file:
```
// Create a Stripe client.
const stripe = Stripe('YOUR_PK_KEY_HERE');
// Create an instance of Elements.
const elements = stripe.elements();

// Some default styling to be used for the form
let style = {
  base: {
    color: '#32325d',
    fontFamily: '"Helvetica Neue", Helvetica, sans-serif',
    fontSmoothing: 'antialiased'
  },
  invalid: {
    color: '#fa755a',
    iconColor: '#fa755a'
  }
};

// Create an instance of the card Element.
let card = elements.create('card', {style: style});

// Add an instance of the card Element into the `card-element` <div>.
card.mount('#card-element');

// Handle real-time validation errors from the card Element.
card.addEventListener('change', function(event) {
  let displayError = document.getElementById('card-errors');
  if (event.error) {
    displayError.textContent = event.error.message;
  } else {
    displayError.textContent = '';
  }
});

// Handle form submission.
let form = document.getElementById('payment-form');
form.addEventListener('submit', function(event) {
  event.preventDefault();
  stripe.createToken(card).then(function(result) {
    if (result.error) {
      // Inform the user if there was an error.
      let errorElement = document.getElementById('card-errors');
      errorElement.textContent = result.error.message;
    } else {
      // Send the token to your server.
      stripeTokenHandler(result.token);
    }
  });
});

// Submit the form with the token ID.
function stripeTokenHandler(token) {
  // Insert the token ID into the form so it gets submitted to the server
  let form = document.getElementById('payment-form');
  let hiddenInput = document.createElement('input');
  hiddenInput.setAttribute('type', 'hidden');
  hiddenInput.setAttribute('name', 'stripeToken');
  hiddenInput.setAttribute('value', token.id);
  form.appendChild(hiddenInput);
  // Submit the form
  form.submit();
}
```
Stripe allows you to use a customer data object, which would be used as a second argument to stripe.createToken function above. For example `stripe.createToken(card, customerData).then(function(result)` where customer data would be something like: `let customerData {name: 'Firstname Lastname', address_line1: '11 Street'}`

Instead I opted to use the Stripe API via PHP to first create a customer in stripe, and then to charge that customer. This worked better for my requirements, as I then had the customer section populating in the backend of Stripe, making it better for reporting. 

Here is the paymentform.php:
```
<?php 

// Fire up composer
require_once('vendor/autoload.php');

// Set your secret key here
\Stripe\Stripe::setApiKey("YOUR_SECRET_KEY");

// Get the payment token ID submitted by the form:
$token = $_POST['stripeToken'];

// Backup of user info to send as meta data
$user_info = array("Name" => $_POST['name'], "Email" => $_POST['email']);

try {
// Createa a customer in stripe
  $customer = \Stripe\Customer::create(array(
    'email' => $_POST['email'],
    'card'  => $token,
  ));
  
// Charge the customer 
    $charge = \Stripe\Charge::create([
        'customer' => $customer->id,
        'amount' => ($_POST['amount']*100),
        'currency' => 'cad',
        'description' => 'Your Service: ' . $_POST['name'],
        'receipt_email' => $_POST['email'],
        'metadata' => $user_info,
    ]);

    // Redirect to thank you page after completing
    header('Location: /thank-you/');

    // Error handling
  } catch(\Stripe\Error\Card $e) {
    // Since it's a decline, \Stripe\Error\Card will be caught
    $body = $e->getJsonBody();
    $err  = $body['error'];

    session_start();
    // Pass the error to session for error handling

    $_SESSION['payment_error_msg'] = $err['message'];
    header('Location: /payment-unsuccessful/');
    ...
  }
```
For some slight brevity, I haven't included the full source code above, the rest is some more error handling, which you can find on the Git Repo. 

I have a try catch, that first creates a customer with the form details, then it submits this new customer a charge, where the amount is taken from the form (and then x 100 to become currency). A dynamic description is added, along with the receipt_email param, which will allow a receipt to be sent to the client. 

As a backup, I have also saved the user information from the form as metadata. This isn't necessary but it does mean you have two places in the backend of Stripe where the customer information is stored. If this is successful it passes through to the thank you page. 

If there is an error then this is handled by saving the error message in a $_SESSION, and then relocating to the unsuccessful error page, where it advises the client what the issue was with the payment.

Hopefully that may help others with creating a custom amount field, or customizing their checkout.

## Further reading

[Stripe.js Docs](https://stripe.com/docs/stripe-js/reference)

[Stripe Elements](https://stripe.com/docs/stripe-js/elements/quickstart)
