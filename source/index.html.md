---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - code

toc_footers:

includes:

search: true
---

# What is a webhook?

A webhook is the act of Zilra making an HTTP POST request a <b>Return URL</b> you have provided when an event occurs. This way Zilra is able to immediately notify you when an event occurs 

# Protecting your webhook

For Zilra to communicate with your application, you will need a publicly accessible URL. You’ll want to protect this URL so that malicious actors cannot manipulate your data. You can add this link to your account by going to the ‘Product’ section under ‘Settings’


# Authentication (Optional)

An easy way to protect your API is through basic HTTP authentication. Almost all web servers can be configured to require a user name and password for access to a URL. You can configure your webhook URL with basic HTTP authentication by adding the user name and password to the URL https://example.com/webhook in the following format and setting the result as the webhook

Example URL - https://&lt;username&gt;:&lt;password&gt;@example.com/webhook

# What is Product Webhook?

> Return

```code
(
    [order_date] => 1587641497
    [order_no] => a30d51
    [mode_of_payment] => Card
    [last4] => 1111
    [order_amount] => 30.00
    [order_transaction_id] => b7de8b5a
    [email_address] => customeremailaddress@domain.com
    [customer_name] => First Name Last Name
    [order_quantity] => 3
    [customer_message] => This is message from your customer to you
    [recurring] => true
    [recurring_cycle] => Monthly
    [note] => UniqueCodeFromYourBusiness
)
```

The Product webhook is a way for Zilra to notify your application that a product has been purchased successfully. This can be useful in a variety of situations, one of the most common use case being giving access instant access to customers for their purchase / subscription / account.

In Zilra, a product is considered as purchased successfully when the payment processing either through Online Banking or Credit/ Debit Card returns a positive response. When that happens, Zilra marks the particular sale as complete. At the same time, if the webhook is enabled, a hook is posted to your <b>Return URL</b>.
### API Reference
Property name | Type | Description
--------- | ------- | -----------
order_date | Integer | Time at which the object was created. Measured in seconds since the Unix epoch. 
order_no | String | Unique identified for the order
mode_of_payment | String | The payment mode used by the customer to complete the order. (Card or Bank)
last4 | Integer | The last four digits of the card or the bank account number
order_amount | Integer | amount collected for this particular order
order_transaction_id | String | Unique identified for every transaction in an account
email_address | String | The email address of the customer placing the order
customer_name | String | The customer’s full name or business name provided at the time of purchase
order_quantity | Integer | The total number of items purchased by the customer in the particular order
customer_message | String (Optional) | A message / order note provided by the customer at the time of placing the order
recurring | Boolean | If the order placed is a subscription agreed by the customer to be charged on the same payment instrument for the same payment amount at an agreed interval.
recurring_cycle | String | The agreed upon interval at which the customer’s payment instrument will be charged automatically.
note | String (Optional) | A message / order note / ID provided by you at the time of redirecting customers to Zilra to identify this specific order on completion.
# Metered Billing

This is a simplified version of metered billing useful in cases where you want to charge your customers the same amount as their standard monthly subscription fee when your system identifies their consumption being high and possible termination of their access before the completion of the billing period.

For example, if you are running a SaaS business that sells a certain number of API call for every package, you identify a customer’s account running low on API Call Credits, you then use Metered Billing to automatically charge them a renewal fee and replenish their account with additional to keep their business going.
  
## Charging on Metered Billing

> URL

```code
https://www.zilra.co/product-api/{apikey}/{secretkey}/{orderno}
```

> Example:

```code
https://www.zilra.co/product-api/msjwrm7o4Wke/d55aae5fed7d888311c3/a30d51
```

>Response - A response as per our ‘Product Webhook’ will be posted to the Return URL you have set in your ‘Product’ settings page.

Name | Description
--------- | -----------
apikey | When logged into Zilra, select ‘Settings’ then scroll down to ‘Product’, scroll down to ‘Production Credentials’ and there you will find your API Key.
secretkey | When logged into Zilra, select ‘Settings’ then scroll down to ‘Product’, scroll down to ‘Production Credentials’ and then click on ‘Show Secret Key’ to see your Secret Key
orderno | Find the order for which you want to charge the customer, use the unique order number. This will work only for order numbers provided for ‘Subscription’ products.