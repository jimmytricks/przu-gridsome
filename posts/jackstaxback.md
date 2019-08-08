---
title: JacksTaxBack.com
date: 2017-09-14
description: A WordPress site, with modified theme and custom Stripe integration (PHP and cURL)
slug: jackstaxback
website: true
websiteurl: https://jackstaxback.com
tech: ["WordPress", "PHP", "cURL", "Stripe"]
featureimg: "./imgs/jtb.png"
---

## Overview

JacksTaxBack.com was built from a WordPress theme, utilizing forms and custom fields.

## Stripe Integration

I also built Stripe integration into the site, using [Stripe's Elements](https://stripe.com/docs/stripe-js/elements/quickstart). This was required due to the nature of requiring the client to input a custom amount to pay, as they varied depending on the type of service they received. 

There was nothing currently available from Stripe that would meet everything the client desired, so in PHP and cURL I implemented a custom approach that also creates a Customer in the backend of Stripe for easy management. 

For more information on the integration and how I achieved it, checkout the [Github](https://github.com/jimmytricks/stripe-custom-amount) for the code. 