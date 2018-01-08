---
layout: twoColumn
section: guides
type: guide
guide:
    name: receive-money
    step: '1'
title:  "Step 1: Customer onboarding"
description: Leverage Dwolla's ach payment API to receive money via bank transfer.
---
# Step 1: Create a Customer and transfer

## Step 1A: Obtain an application access token

Your application will exchange its `client_id`, `client_secret`, and `grant_type=client_credentials` for an application access token. An application access token can then be used to make calls to the Dwolla API on behalf of your application.

## Step 1B: Create a Customer

Create a Customer for the user that is going to pay you. At a minimum, provide the user’s full name and email address to create the customer.

```raw
POST https://api-sandbox.dwolla.com/customers
Content-Type: application/vnd.dwolla.v1.hal+json
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer 0Sn0W6kzNicvoWhDbQcVSKLRUpGjIdlPSEYyrHqrDDoRnQwE7Q

{
"firstName": "Joe",
"lastName": "Buyer",
"email": "jbuyer@mail.net",
"ipAddress": "99.99.99.99"
}

HTTP/1.1 201 Created
Location: https://api-sandbox.dwolla.com/customers/247b1bd8-f5a0-4b71-a898-f62f67b8ae1c
```

```ruby
request_body = {
  :firstName => 'Joe',
  :lastName => 'Buyer',
  :email => 'jbuyer@mail.net',
  :ipAddress => '99.99.99.99'
}

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
new_customer = app_token.post "customers", request_body
new_customer.response_headers[:location] # => "https://api-sandbox.dwolla.com/customers/247b1bd8-f5a0-4b71-a898-f62f67b8ae1c"

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
  .then(function(res) {
    res.headers.get('location'); // => 'https://api-sandbox.dwolla.com/customers/247b1bd8-f5a0-4b71-a898-f62f67b8ae1c'
  });
```

```python
request_body = {
  'firstName': 'Joe',
  'lastName': 'Buyer',
  'email': 'jbuyer@mail.net',
  'ipAddress': '99.99.99.99'
}

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
new_customer = app_token.post('customers', request_body)
new_customer.headers['location'] # => 'https://api-sandbox.dwolla.com/customers/247b1bd8-f5a0-4b71-a898-f62f67b8ae1c'

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

print($new_customer); # => https://api-sandbox.dwolla.com/customers/247b1bd8-f5a0-4b71-a898-f62f67b8ae1c
?>
```

```java
CustomersApi cApi = new CustomersApi(a);

CreateCustomer newCustomerData = new CreateCustomer();

myNewCust.setFirstName("Joe");
myNewCust.setLastName("Buyer");
myNewCust.setEmail("jbuyer@mail.com");
myNewCust.setIpAddress("99.99.99.99");

try {
    Unit$ r = cApi.create(myNewCust);
    System.out.println(r.getLocationHeader()); // => https://api-sandbox.dwolla.com/customers/247B1BD8-F5A0-4B71-A898-F62F67B8AE1C
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

## Step 1C: Attach a funding source to the Customer

Next you will attach a verified funding source to the Customer, which will be done using Instant Account Verification (IAV). This method will give the Customer the ability to add and verify their bank account in a matter of seconds by authenticating with their online banking credentials. Once the Customer reaches the page in your application to add a bank account you'll ask Dwolla’s server to [generate an IAV token](http://docsv2.dwolla.com/#generate-an-iav-token).

Generate a single-use IAV token for our Customer:

```raw
curl -X POST
\ -H "Content-Type: application/vnd.dwolla.v1.hal+json"
\ -H "Accept: application/vnd.dwolla.v1.hal+json"
\ -H "Authorization: Bearer 0Sn0W6kzNicvoWhDbQcVSKLRUpGjIdlPSEYyrHqrDDoRnQwE7Q"
\ "https://api-sandbox.dwolla.com/customers/247b1bd8-f5a0-4b71-a898-f62f67b8ae1c/iav-token"

HTTP/1.1 200 OK
{
   "_links":{
      "self":{
         "href":"https://api-sandbox.dwolla.com/customers/247b1bd8-f5a0-4b71-a898-f62f67b8ae1c/iav-token"
      }
   },
   "token":"lr0Ax1zwIpeXXt8sJDiVXjPbwEeGO6QKFWBIaKvnFG0Sm2j7vL"
}
```

```ruby
customer_url = 'https://api-sandbox.dwolla.com/customers/247b1bd8-f5a0-4b71-a898-f62f67b8ae1c'

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
customer = app_token.post "#{customer_url}/iav-token"
customer.token # => "lr0Ax1zwIpeXXt8sJDiVXjPbwEeGO6QKFWBIaKvnFG0Sm2j7vL"
```

```javascript
// Using dwolla-v2 - https://github.com/Dwolla/dwolla-v2-node
var customerUrl = 'https://api-sandbox.dwolla.com/customers/247b1bd8-f5a0-4b71-a898-f62f67b8ae1c';

appToken
  .post(`${customerUrl}/iav-token`)
  .then(function(res) {
    res.body.token; // => 'lr0Ax1zwIpeXXt8sJDiVXjPbwEeGO6QKFWBIaKvnFG0Sm2j7vL'
  });
```

```python
customer_url = 'http://api.dwolla.com/customers/247b1bd8-f5a0-4b71-a898-f62f67b8ae1c'

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
customer = app_token.post('%s/iav-token' % customer_url)
customer.body['token'] # => 'lr0Ax1zwIpeXXt8sJDiVXjPbwEeGO6QKFWBIaKvnFG0Sm2j7vL'
```

```php
<?php
$customersApi = new DwollaSwagger\CustomersApi($apiClient);

$fsToken = $customersApi->getCustomerIavToken("https://api-sandbox.dwolla.com/customers/247b1bd8-f5a0-4b71-a898-f62f67b8ae1c");
$fsToken->token; # => "lr0Ax1zwIpeXXt8sJDiVXjPbwEeGO6QKFWBIaKvnFG0Sm2j7vL"
?>
```

Then, you’ll pass this single-use IAV token to the client-side of your application where it will be used in the JavaScript function `dwolla.iav.start`. This token will be used to authenticate the request asking Dwolla to render the IAV flow. Before calling this function you'll want to include `dwolla.js` in the HEAD of your page.

```htmlnoselect
<head>
<script src="https://cdn.dwolla.com/1/dwolla.js"></script>
</head>
```

Next, you'll add in a container to the body of your page where you want to render the IAV flow.

```htmlnoselect
<div id="mainContainer">
  <input type="button" id="start" value="Add Bank">
</div>

<div id="iavContainer"></div>
```

Now that you have `dwolla.js` initialized on the page and the container created where you'll render the IAV flow, you'll create a JavaScript function that responds to the Customer clicking the "Add bank" button on your page. Once the Customer clicks "Add Bank", your application will call `dwolla.iav.start()` passing in the following arguments: a string value of your single-use IAV token, options such as the iavContainer element where IAV will render, and a callback function that will handle any error or response.

```javascriptnoselect
<script type="text/javascript">
$('#start').click(function() {
  var iavToken = '4adF858jPeQ9RnojMHdqSD2KwsvmhO7Ti7cI5woOiBGCpH5krY';
  dwolla.configure('sandbox');
  dwolla.iav.start(iavToken, {
          container: 'iavContainer',
          stylesheets: [
            'http://fonts.googleapis.com/css?family=Lato&subset=latin,latin-ext',
            'http://localhost:8080/iav/customStylesheet.css'
          ],
          microDeposits: 'true',
          fallbackToMicroDeposits: (fallbackToMicroDeposits.value === 'true')
        }, function(err, res) {
    console.log('Error: ' + JSON.stringify(err) + ' -- Response: ' + JSON.stringify(res));
  });
});
</script>
```

The customer will complete the IAV flow by authenticating with their online banking credentials. You'll know their bank account was successfully added and verified if you receive a JSON response in your callback that includes a link to the newly created funding source.

* Sample response:  `{"_links":{"funding-source":{"href":"https://api-sandbox.dwolla.com/funding-sources/80275e83-1f9d-4bf7-8816-2ddcd5ffc197"}}}`

Great! The funding source should now be verified.

## Step 1D: Create a transfer

Once the customer has verified their funding source, we can transfer funds from their bank account to your Dwolla account. You’ll need to supply your access token from step A, the customer’s ID from step B, and the customer’s funding source ID from step C:

```raw
POST https://api-sandbox.dwolla.com/transfers
Accept: application/vnd.dwolla.v1.hal+json
Content-Type: application/vnd.dwolla.v1.hal+json
Authorization: Bearer 0Sn0W6kzNicvoWhDbQcVSKLRUpGjIdlPSEYyrHqrDDoRnQwE7Q
{
    "_links": {
        "source": {
            "href": "https://api-sandbox.dwolla.com/funding-sources/80275e83-1f9d-4bf7-8816-2ddcd5ffc197"
        },
        "destination": {
            "href": "https://api-sandbox.dwolla.com/accounts/ab443d36-3757-44c1-a1b4-29727fb3111c"
        }
    },
    "amount": {
        "currency": "USD",
        "value": "225.00"
    }
}

HTTP/1.1 201 Created
Location: https://api-sandbox.dwolla.com/transfers/d76265cd-0951-e511-80da-0aa34a9b2388
```

```ruby
request_body = {
  :_links => {
    :source => {
      :href => "https://api-sandbox.dwolla.com/funding-sources/80275e83-1f9d-4bf7-8816-2ddcd5ffc197"
    },
    :destination => {
      :href => "https://api-sandbox.dwolla.com/accounts/ab443d36-3757-44c1-a1b4-29727fb3111c"
    }
  },
  :amount => {
    :currency => "USD",
    :value => "225.00"
  },
  :metadata => {
    :foo => "bar",
    :baz => "boo"
  }
}

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
transfer = app_token.post "transfers", request_body
transfer.response_headers[:location] # => "https://api.dwolla.com/transfers/d76265cd-0951-e511-80da-0aa34a9b2388"
```

```javascript
var requestBody = {
  _links: {
    source: {
      href: 'https://api-sandbox.dwolla.com/funding-sources/80275e83-1f9d-4bf7-8816-2ddcd5ffc197'
    },
    destination: {
      href: 'https://api-sandbox.dwolla.com/accounts/ab443d36-3757-44c1-a1b4-29727fb3111c'
    }
  },
  amount: {
    currency: 'USD',
    value: '225.00'
  },
  metadata: {
    foo: 'bar',
    baz: 'boo'
  }
};

appToken
  .post('transfers', requestBody)
  .then(function(res) {
    res.headers.get('location'); // => 'https://api.dwolla.com/transfers/d76265cd-0951-e511-80da-0aa34a9b2388'
  });
```

```python
request_body = {
  '_links': {
    'source': {
      'href': 'https://api-sandbox.dwolla.com/funding-sources/80275e83-1f9d-4bf7-8816-2ddcd5ffc197'
    },
    'destination': {
      'href': 'https://api-sandbox.dwolla.com/accounts/ab443d36-3757-44c1-a1b4-29727fb3111c'
    }
  },
  'amount': {
    'currency': 'USD',
    'value': '225.00'
  },
  'metadata': {
    'foo': 'bar',
    'baz': 'boo'
  }
}

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
transfer = app_token.post('transfers', request_body)
transfer.headers['location'] # => 'https://api.dwolla.com/transfers/d76265cd-0951-e511-80da-0aa34a9b2388'
```

```php
<?php
$transfer_request = array (
  '_links' =>
  array (
    'source' =>
    array (
      'href' => 'https://api-sandbox.dwolla.com/funding-sources/80275e83-1f9d-4bf7-8816-2ddcd5ffc197',
    ),
    'destination' =>
    array (
      'href' => 'https://api-sandbox.dwolla.com/accounts/ab443d36-3757-44c1-a1b4-29727fb3111c',
    ),
  ),
  'amount' =>
  array (
    'currency' => 'USD',
    'value' => '225.00',
  )
);

$transferApi = new DwollaSwagger\TransfersApi($apiClient);
$myAccount = $transferApi->create($transfer_request);

print($xfer); # => https://api-sandbox.dwolla.com/transfers/d76265cd-0951-e511-80da-0aa34a9b2388
?>
```

<nav class="pager-nav">
    <a href="./">Back: Overview</a>
    <a href="check-transfer.html">Next step: Check transfer status</a>
</nav>
