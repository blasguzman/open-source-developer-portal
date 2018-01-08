---
layout: twoColumn
section: guides
guide:
    name: send-money
    step: '1'
type: guide
title:  "Step 1: Customer onboarding"
description: Use Dwolla's ACH payment API to send money to other users.
---
# Step 1: Create recipients using the Dwolla API

In this experience, end users create their accounts entirely within your application and you prompt for their bank or credit union account information. Dwolla will securely store this sensitive information.

## Step 1A. Obtain an application access token

Your application will exchange its `client_id`, `client_secret`, and `grant_type=client_credentials` for an [application access token](https://docsv2.dwolla.com/#application-authorization). An application access token can then be used to make calls to the API on behalf of your application.

## Step 1B. Create a Customer

Create a Customer for each user you’d like to transfer funds to. At a minimum, provide the user’s full name, and email address to create the Customer. More detail is available in [API docs](https://docsv2.dwolla.com/#create-a-customer). For the purpose of this guide we'll be creating the `Unverified Customer` type, however if your use case doesn't require the need for your user to send funds then the `Receive-only Customer` type is recommended.

```raw
POST https://api-sandbox.dwolla.com/customers
Content-Type: application/vnd.dwolla.v1.hal+json
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer 0Sn0W6kzNicvoWhDbQcVSKLRUpGjIdlPSEYyrHqrDDoRnQwE7Q
{
  "firstName": "Jane",
  "lastName": "Merchant",
  "email": "jmerchant@nomail.net",
  "ipAddress": "99.99.99.99"
}

HTTP/1.1 201 Created
Location: https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747
```

```ruby
request_body = {
  :firstName => 'Jane',
  :lastName => 'Merchant',
  :email => 'jmerchant@nomail.net',
  :ipAddress => '99.99.99.99'
}

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
customer = app_token.post "customers", request_body
customer.response_headers[:location] # => "https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747"
```

```javascript
var requestBody = {
  firstName: 'Jane',
  lastName: 'Merchant',
  email: 'jmerchant@nomail.net',
  ipAddress: '99.99.99.99'
};

appToken
  .post('customers', requestBody)
  .then(function(res) {
    res.headers.get('location'); // => 'https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747'
  });
```

```python
request_body = {
  'firstName': 'Jane',
  'lastName': 'Merchant',
  'email': 'jmerchant@nomail.net',
  'ipAddress': '99.99.99.99'
}

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
customer = app_token.post('customers', request_body)
customer.headers['location'] # => 'https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747'
```

```php
<?php
$customersApi = new DwollaSwagger\CustomersApi($apiClient);

$customer = $customersApi->create([
  'firstName' => 'Jane',
  'lastName' => 'Merchant',
  'email' => 'jmerchant@nomail.net',
  'ipAddress' => '99.99.99.99'
]);

print($customer); # => "https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747"
?>
```

```java
CustomersApi cApi = new CustomersApi(a);

CreateCustomer newCustomerData = new CreateCustomer();

myNewCust.setFirstName("Jane");
myNewCust.setLastName("Merchant");
myNewCust.setEmail("jmerchant@nomail.net");
myNewCust.setIpAddress("99.99.99.99");

try {
    Unit$ r = cApi.create(myNewCust);
    System.out.println(r.getLocationHeader()); // => https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747
}
catch (Exception e) {
    System.out.println("Something's up!");
}
```

When the Customer is created, you’ll receive the Customer URL in the location header.

<ol class = "alerts">
    <li class="alert icon-alert-info">
      Provide the IP address of the end-user accessing your application as the `ipAddress` parameter. This enhances fraud detection and tracking.
    </li>
</ol>

Looking to learn more about each Customer type and how it relates to your funds flow? Take a look at our [Account types article](https://developers.dwolla.com/resources/account-types.html) for more information.

### Step 1C. Attach a funding source to the Customer

The next step is to attach a bank or credit union account to the Customer by providing the bank account’s routing number, account number, account type, and an arbitrary name.

Funds transferred to this Customer will be automatically swept into the funding source. The example below shows sample bank information, but you will include actual routing, account, and bank name after prompting your customer for this information within your application. Possible values for `type` can be either “checking” or “savings”. More detail is available in [API docs](https://docsv2.dwolla.com/#create-a-funding-source-for-a-customer).

```raw
POST https://api.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747/funding-sources
Content-Type: application/vnd.dwolla.v1.hal+json
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer 0Sn0W6kzNicvoWhDbQcVSKLRUpGjIdlPSEYyrHqrDDoRnQwE7Q
{
    "routingNumber": "222222226",
    "accountNumber": "123456789",
    "bankAccountType": "checking",
    "name": "Jane Merchant - Checking 6789"
}

HTTP/1.1 201 Created
Location: https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31
```

```ruby
customer_url = 'https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747'
request_body = {
  routingNumber: '222222226',
  accountNumber: '123456789',
  bankAccountType: 'checking',
  name: 'Jane Merchant - Checking 6789'
}

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
funding_source = account_token.post "#{customer_url}/funding-sources", request_body
funding_source.response_headers[:location] # => "https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31"

```

```javascript
var customerUrl = 'https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747';
var requestBody = {
  'routingNumber': '222222226',
  'accountNumber': '123456789',
  'bankAccountType': 'checking',
  'name': 'Jane Merchant - Checking 6789'
};

accountToken
  .post(`${customerUrl}/funding-sources`, requestBody)
  .then(function(res) {
    res.headers.get('location'); // => 'https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31'
  });
```

```python
customer_url = 'https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747'
request_body = {
  'routingNumber': '222222226',
  'accountNumber': '123456789',
  'bankAccountType': 'checking',
  'name': 'Jane Merchant - Checking 6789'
}

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
customer = account_token.post('%s/funding-sources' % customer_url, request_body)
customer.headers['location'] # => 'https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31'

```

```php
<?php
$fundingApi = new DwollaSwagger\FundingsourcesApi($apiClient);

$new_fs = $fundingApi->createCustomerFundingSource([
  "routingNumber" => "222222226",
  "accountNumber" => "123456789",
  "bankAccountType" => "checking",
  "name" => "Jane Merchant - Checking 6789"
], "https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747");

print($new_fs); # => https://api-sandbox.dwolla.com/funding-sources/375c6781-2a17-476c-84f7-db7d2f6ffb31
?>
```

The created funding source URL is returned in the location header.

<nav class="pager-nav">
    <a href="./">Back: Overview</a>
    <a href="fetch-funding-sources.html">Next step: Fetch funding sources</a>
</nav>
