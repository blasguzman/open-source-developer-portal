---
layout: twoColumn
section: guides
type: guide
guide:
    name: transfer-money-between-users
    step: '2'
title:  "Step 2: Create a verified customer"
description: Create a verified customer within your Dwolla API application.
---
# Step 2: Create a Verified Customer

First, we’ll create a `Verified Customer` for Jane Merchant.

The following information is required for a `Verified Customer`. In this example, we use business verified customers to represent the merchant who will be receiving funds.

```raw
POST https://api-sandbox.dwolla.com/customers
Content-Type: application/vnd.dwolla.v1.hal+json
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer 0Sn0W6kzNicvoWhDbQcVSKLRUpGjIdlPSEYyrHqrDDoRnQwE7Q
{
  "firstName": "Jane",
  "lastName": "Merchant",
  "email": "janeMerchant@email.com",
  "ipAddress": "127.0.0.1",
  "type": "business",
  "address1": "99-99 33rd St",
  "city": "Some city",
  "state": "NY",
  "postalCode": "11101",
  "dateOfBirth": "1970-01-01",
  "ssn": "1234",
  "businessClassification": "9ed38155-7d6f-11e3-83c3-5404a6144203",
  "businessType": "llc",
  "businessName":"Jane Corp",
  "ein":"12-3456789"
}

HTTP/1.1 201 Created
Location: https://api-sandbox.dwolla.com/customers/AB443D36-3757-44C1-A1B4-29727FB3111C
```

```ruby
request_body = {
  :firstName => 'Jane',
  :lastName => 'Merchant',
  :email => 'janeMerchant@email.com',
  :type => 'business',
  :address1 => '99-99 33rd St',
  :city => 'Some City',
  :state => 'NY',
  :postalCode => '11101',
  :dateOfBirth => '1970-01-01',
  :ssn => '1234',
  :businessClassification => '9ed38155-7d6f-11e3-83c3-5404a6144203',
  :businessType => 'llc',
  :businessName => 'Jane Corp',
  :ein => '12-3456789',
}
# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
customer = app_token.post "customers", request_body
customer.response_headers[:location] # => "https://api-sandbox.dwolla.com/customers/AB443D36-3757-44C1-A1B4-29727FB3111C"
```

```javascript
var requestBody = {
  firstName: 'Jane',
  lastName: 'Merchant',
  email: 'janeMerchant@email.com',
  type: 'business',
  address1: '99-99 33rd St',
  city: 'Some City',
  state: 'NY',
  postalCode: '11101',
  dateOfBirth: '1970-01-01',
  ssn: '1234',
  businessClassification: '9ed38155-7d6f-11e3-83c3-5404a6144203',
  businessType: 'llc',
  businessName: 'Jane Corp',
  ein: '12-3456789'
};

appToken
  .post('customers', requestBody)
  .then(res => res.headers.get('location')); // => 'https://api-sandbox.dwolla.com/customers/AB443D36-3757-44C1-A1B4-29727FB3111C'
```

```python
request_body = {
  'firstName': 'Jane',
  'lastName': 'Merchant',
  'email': 'janeMerchant@email.com',
  'type': 'business',
  'address1': '99-99 33rd St',
  'city': 'Some City',
  'state': 'NY',
  'postalCode': '11101',
  'dateOfBirth': '1970-01-01',
  'ssn': '1234',
  'businessClassification': '9ed38155-7d6f-11e3-83c3-5404a6144203',
  'businessType': 'llc',
  'businessName': 'Jane Corp',
  'ein': '12-3456789'
}

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
customer = app_token.post('customers', request_body)
customer.headers['location'] # => 'https://api-sandbox.dwolla.com/customers/AB443D36-3757-44C1-A1B4-29727FB3111C'
```

```php
<?php
$customersApi = new DwollaSwagger\CustomersApi($apiClient);

$new_customer = $customersApi->create([
  'firstName' => 'Jane',
  'lastName' => 'Merchant',
  'email' => 'janeMerchant@email.com',
  'type' => 'business',
  'address1' => '99-99 33rd St',
  'city' => 'Some City',
  'state' => 'NY',
  'postalCode' => '11101',
  'dateOfBirth' => '1970-01-01',
  'ssn' => '1234',
  'businessClassification' => '9ed38155-7d6f-11e3-83c3-5404a6144203',
  'businessType' => 'llc',
  'businessName' => 'Jane Corp',
  'ein' => '12-3456789'
]);

print($new_customer); # => https://api-sandbox.dwolla.com/customers/AB443D36-3757-44C1-A1B4-29727FB3111C
?>
```

```java
CustomersApi cApi = new CustomersApi(a);

CreateCustomer newCustomerData = new CreateCustomer();

myNewCust.setFirstName("Jane");
myNewCust.setLastName("Merchant");
myNewCust.setEmail("janeMerchant@email.com");
myNewCust.setType("personal");
myNewCust.setAddress("99-99 33rd St");
myNewCust.setCity("Some City");
myNewCust.setState("NY");
myNewCust.setPostalCode("11101");
myNewCust.setDateOfBirth("1970-01-01");

try {
    Unit$ r = cApi.create(myNewCust);
    System.out.println(r.getLocationHeader()); // => https://api-sandbox.dwolla.com/customers/AB443D36-3757-44C1-A1B4-29727FB3111C
}
catch (Exception e) {
    System.out.println("Something's up!");
}
```

When the customer is created, you’ll receive the customer URL in the location header.

**Important:** There are various reasons a Verified Customer will result in a status other than `verified` which you will want to account for after the Customer is created. Reference the Customer verification resource article for more information on [handling verification statuses](https://developers.dwolla.com/resources/customer-verification/handling-verification-statuses.html).

<nav class="pager-nav">
    <a href="./obtain-access-token.html">Back: Obtain an access token</a>
    <a href="attach-unverified-bank.html">Next step: Attach an unverified funding source</a>
</nav>
