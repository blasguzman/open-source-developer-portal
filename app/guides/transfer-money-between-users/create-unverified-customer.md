---
layout: twoColumn
section: guides
type: guide
guide:
    name: transfer-money-between-users
    step: '4'
title:  "Step 4: Creating an unverified customer"
description: Create an unverified customer within Dwolla's ACH payment API.
---

# Step 4: Creating an Unverified Customer

Now that we’ve created a customer for Jane Merchant and associated a funding source, we’ll do the same for Joe Buyer, but this time we’ll create an `Unverified Customer`, and a verified funding source which is capable of sending money.

Provide the user’s full name, email address, and IP address to create the Customer. More detail is available in [API docs](https://docsv2.dwolla.com/#customers). 

```raw
POST https://api-sandbox.dwolla.com/customers
Content-Type: application/vnd.dwolla.v1.hal+json
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer 0Sn0W6kzNicvoWhDbQcVSKLRUpGjIdlPSEYyrHqrDDoRnQwE7Q
{
"firstName": "Joe", 
"lastName": "Buyer",
"email": "jbuyer@mail.net"
}

HTTP/1.1 201 Created
Location: https://api-sandbox.dwolla.com/customers/247B1BD8-F5A0-4B71-A898-F62F67B8AE1C
```
```ruby
request_body = {
  :firstName => 'Joe',
  :lastName => 'Buyer',
  :email => 'jbuyer@mail.net',
  :ipAddress => '99.99.99.99'
}

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
customer = app_token.post "customers", request_body
customer.response_headers[:location] # => "https://api-sandbox.dwolla.com/customers/247B1BD8-F5A0-4B71-A898-F62F67B8AE1C"

```
```javascript
var requestBody = {
  firstName: 'Joe',
  lastName: 'Buyer',
  email: 'jbuyer@mail.net',
  ipAddress: '99.99.99.99'
};

appToken
  .post('customers', requestBody)
  .then(res => res.headers.get('location')); // => 'https://api-sandbox.dwolla.com/customers/247B1BD8-F5A0-4B71-A898-F62F67B8AE1C'
```
```python
request_body = {
  'firstName': 'Joe',
  'lastName': 'Buyer',
  'email': 'jbuyer@mail.net',
  'ipAddress': '99.99.99.99'
}

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
customer = app_token.post('customers', request_body)
customer.headers['location'] # => 'https://api-sandbox.dwolla.com/customers/247B1BD8-F5A0-4B71-A898-F62F67B8AE1C'
```
```php
<?php
$customersApi = new DwollaSwagger\CustomersApi($apiClient);

$new_customer = $customersApi->create([
  'firstName' => 'Joe',
  'lastName' => 'Buyer',
  'email' => 'jbuyer@mail.net',
  'ipAddress' => '99.99.99.99'
]);

print($new_customer); # => https://api-sandbox.dwolla.com/customers/247B1BD8-F5A0-4B71-A898-F62F67B8AE1C
?>
```
```java
CustomersApi cApi = new CustomersApi(a);

CreateCustomer newCustomerData = new CreateCustomer();

myNewCust.setFirstName("Joe");
myNewCust.setLastName("Buyer");
myNewCust.setEmail("jbuyer@mail.com");

try {
    Unit$ r = cApi.create(myNewCust);
    System.out.println(r.getLocationHeader()); // => https://api-sandbox.dwolla.com/customers/247B1BD8-F5A0-4B71-A898-F62F67B8AE1C
}
catch (Exception e) {
    System.out.println("Something's up!");
}
```

<ol class = "alerts">
    <li class="alert icon-alert-info">
      Provide the IP address of the end-user accessing your application as the `ipAddress` parameter. This enhances fraud detection and tracking.
    </li>
</ol>

When the customer is created, you’ll receive the customer URL in the location header.

<nav class="pager-nav">
    <a href="./attach-unverified-bank.html">Back: Attach an unverified funding source</a>
    <a href="attach-verified-bank.html">Next step: Attach a verified funding source</a>
</nav>
