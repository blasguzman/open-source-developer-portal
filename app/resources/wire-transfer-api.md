---
layout: twoColumn
section: resources
type: features
title:  "Wire transfer API"
description: "A walkthrough of sending wire transfers through the Dwolla API."
---

# Overview
Wire transfers continue to be a staple functionality in the world of B2B and B2C commerce, so we’ve made the ability to wire funds out of the Dwolla platform available to Dwolla API partners. In some instances, wire transfers may prove beneficial to Dwolla API partners over standard ACH; for example, they provide the ability to move large transactions out of the Dwolla platform at a faster speed. In this article we'll cover how to wire out funds to a bank from the Dwolla platform.

A few key items to note are:

*  Funds are available the **same business day**, rather than days later.
*  Wire transfers are more expensive than ACH transfers to send and receive.
*  A wire transfer can only be facilitated out of the Dwolla platform to a bank for a Dwolla API partner account or its [Verified Customers](/resources/account-types.html).
*  Funds can only be wired to customers with domestic (U.S.) bank accounts.

#### Wire transfer processing times and events
Your application can initiate a wire transfer from a `balance` funding source prior to **3:00PM Central Time** in order for the funds to be available in the destination bank account that same day. If your application has an active [webhook subscription](/guides/webhooks/create-subscription.html), [events](https://docsv2.dwolla.com/#events) will be triggered for when the transfer is initiated (`customer_bank_transfer_created`) and when it has completed (`customer_bank_transfer_completed`).

### Creating a Verified Customer and adding a wire funding source
Before you can initiate a wire transfer, you must first have a [Verified Customer created](https://docsv2.dwolla.com/#create-a-customer). Reference our [Customer verification resource article](/resources/customer-verification.html) which describes additional information required to create a CIP verified Customer account. Dwolla will use the name on the created Customer account as the beneficiary, therefore it must match the bank owner’s name. The name on the Customer account will also be displayed on your bank statement when the transfer completes.

#### Adding a wire funding source to a Customer

When attaching a wire bank account to the Customer, you'll need to set the `channels` attribute to `wire` in the request (as shown below). Once your customer has connected a bank account, you'll then want to store the funding source id (e.g. `https://api-sandbox.dwolla.com/funding-sources/ecf993e2-fa22-4cea-8022-c7861200288f`) which will be used when specifying the bank account as the destination funding source in the request to [initiate a transfer](https://docsv2.dwolla.com/#initiate-a-transfer).

##### Request and response (view schema in 'raw')
```raw
POST https://api-sandbox.dwolla.com/customers/99bfb139-eadd-4cdf-b346-7504f0c16c60/funding-sources
Content-Type: application/vnd.dwolla.v1.hal+json
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
{
  "routingNumber": "222222226",
  "accountNumber": "123456789",
  "type": "checking",
  "name": "Jane Doe’s Checking",
  "channels": ["wire"]
}

HTTP/1.1 201 Created
Location: https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31
```
```php
/**
 *  No example for this language yet. Coming soon.
 **/
?>
```
```ruby
customer_url = 'https://api-sandbox.dwolla.com/customers/99bfb139-eadd-4cdf-b346-7504f0c16c60'
request_body = {
  routingNumber: '222222226',
  accountNumber: '123456789',
  type: 'checking',
  name: 'Jane Doe’s Checking',
  channels: [
    'wire'
  ]
}

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
funding_source = app_token.post "#{customer_url}/funding-sources", request_body
funding_source.response_headers[:location] # => "https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31"

```
```python
# No example for this language yet. Coming soon.
```
```javascript
var customerUrl = 'https://api-sandbox.dwolla.com/customers/99bfb139-eadd-4cdf-b346-7504f0c16c60';
var requestBody = {
  'routingNumber': '222222226',
  'accountNumber': '123456789',
  'type': 'checking',
  'name': 'Jane Doe’s Checking',
  'items': [
    'wire'
  ]
};

appToken
  .post(`${customerUrl}/funding-sources`, requestBody)
  .then(res => res.headers.get('location')); // => 'https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31'
```


### Initiating a wire transfer
When initiating the wire transfer out of the Dwolla platform we'll need to specify the *source* as our `balance` and the *destination* as the `wire` funding source we created in the previous step.

In order to obtain the Customer's `balance` funding source, you'll need to fetch the [list of funding sources](https://docsv2.dwolla.com/#list-funding-sources-for-a-customer) for the Customer. The balance funding source should be made available when the Customer has a status of `verified`.

The following example assumes the Customer initiating the wire transfer has funds available in their `balance` from a previous transaction.

##### Request and response (view schema in 'raw')
```raw
POST https://api-sandbox.dwolla.com/transfers
Accept: application/vnd.dwolla.v1.hal+json
Content-Type: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
Idempotency-Key: 19051a62-3403-11e6-ac61-9e71128cae77

{
    "_links": {
        "source": {
            "href": "https://api-sandbox.dwolla.com/funding-sources/b268f6b9-db3b-4ecc-83a2-8823a53ec8b7"
        },
        "destination": {
            "href": "https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31"
        }
    },
    "amount": {
        "currency": "USD",
        "value": "10000.00"
    }
}

...

HTTP/1.1 201 Created
Location: https://api-sandbox.dwolla.com/transfers/636de847-7d02-e711-80ee-0aa34a9b2388
```
```ruby
request_body = {
  :_links => {
    :source => {
      :href => "https://api-sandbox.dwolla.com/funding-sources/b268f6b9-db3b-4ecc-83a2-8823a53ec8b7"
    },
    :destination => {
      :href => "https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31"
    }
  },
  :amount => {
    :currency => "USD",
    :value => "10000.00"
  }
}

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
transfer = app_token.post "transfers", request_body
transfer.response_headers[:location] # => "https://api-sandbox.dwolla.com/transfers/636de847-7d02-e711-80ee-0aa34a9b2388"
```
```php
<?php
$transfersApi = new DwollaSwagger\TransfersApi($apiClient);

$transfer = $transfersApi->create([
  '_links' => [
    'source' => [
      'href' => 'https://api-sandbox.dwolla.com/funding-sources/b268f6b9-db3b-4ecc-83a2-8823a53ec8b7',
    ],
    'destination' => [
      'href' => 'https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31'
    ]
  ],
  'amount' => [
    'currency' => 'USD',
    'value' => '10000.00'
  ]
]);
$transfer; # => "https://api-sandbox.dwolla.com/transfers/636de847-7d02-e711-80ee-0aa34a9b2388"
?>
```
```python
request_body = {
  '_links': {
    'source': {
      'href': 'https://api-sandbox.dwolla.com/funding-sources/b268f6b9-db3b-4ecc-83a2-8823a53ec8b7'
    },
    'destination': {
      'href': 'https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31'
    }
  },
  'amount': {
    'currency': 'USD',
    'value': '10000.00'
  }
}

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
transfer = app_token.post('transfers', request_body)
transfer.headers['location'] # => 'https://api-sandbox.dwolla.com/transfers/636de847-7d02-e711-80ee-0aa34a9b2388'
```
```javascript
var requestBody = {
  _links: {
    source: {
      href: 'https://api-sandbox.dwolla.com/funding-sources/b268f6b9-db3b-4ecc-83a2-8823a53ec8b7'
    },
    destination: {
      href: 'https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31'
    }
  },
  amount: {
    currency: 'USD',
    value: '10000.00'
  }
};

appToken
  .post('transfers', requestBody)
  .then(res => res.headers.get('location')); // => 'https://api.dwolla.com/transfers/636de847-7d02-e711-80ee-0aa34a9b2388'
```

### Wire transfer failures
If the receiving financial institution returns the wire transfer, Dwolla will mark the transfer as `failed` and your application will receive a webhook containing the `customer_bank_transfer_failed` event. Once the transfer failure event is triggered the funds will then be made available in the Customer's balance.
