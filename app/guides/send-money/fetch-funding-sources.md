---
layout: twoColumn
section: guides
type: guide
guide:
    name: send-money
    step: '2'
title:  "Step 2: Fetch funding sources"
description: Fetch funding sources within Dwolla's bank transfer API.
---
# Step 2: Fetch funding sources

Now that you’ve created a Customer and associated its funding source, you can initiate your first transfer. The transfer requires the following information:

- The funding source to pull the funds from (your linked bank account)
- The recipient to push the funds to

Dwolla uses URLs to represent relations between resources. Therefore, you’ll need to provide the full URL of the funding source and recipient.

## Fetch your Account's list of available funding sources

Use the [list an account's funding sources](https://docsv2.dwolla.com/#list-funding-sources-for-an-account) endpoint to fetch a list of your own funding sources. You'll need your account URL which can be retrieved by calling [the Root](https://docsv2.dwolla.com/#root) of the API.

#### Request and response (view schema in 'raw')

```raw
GET https://api-sandbox.dwolla.com/accounts/4BB512E4-AD4D-4F7E-BFD0-A232007F21A1/funding-sources
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer 0Sn0W6kzNicvoWhDbQcVSKLRUpGjIdlPSEYyrHqrDDoRnQwE7Q

{
  "_links": {
    "self": {
      "href": "https://api-sandbox.dwolla.com/accounts/4bb512e4-ad4d-4f7e-bfd0-a232007f21a1/funding-sources"
    }
  },
  "_embedded": {
    "funding-sources": [
      {
        "_links": {
          "self": {
            "href": "https://api-sandbox.dwolla.com/funding-sources/0094b1b4-e171-4dc8-865b-cb121c2377bb"
          },
          "account": {
            "href": "https://api-sandbox.dwolla.com/accounts/4bb512e4-ad4d-4f7e-bfd0-a232007f21a1"
          },
          "with-available-balance": {
            "href": "https://api-sandbox.dwolla.com/funding-sources/0094b1b4-e171-4dc8-865b-cb121c2377bb"
          }
        },
        "id": "0094b1b4-e171-4dc8-865b-cb121c2377bb",
        "status": "verified",
        "type": "balance",
        "name": "Balance",
        "created": "2013-09-07T14:42:52.000Z"
      },
      {
        "_links": {
          "self": {
            "href": "https://api-sandbox.dwolla.com/funding-sources/5cfcdc41-10f6-4a45-b11d-7ac89893d985"
          },
          "account": {
            "href": "https://api-sandbox.dwolla.com/accounts/4bb512e4-ad4d-4f7e-bfd0-a232007f21a1"
          }
        },
        "id": "5cfcdc41-10f6-4a45-b11d-7ac89893d985",
        "status": "verified",
        "type": "bank",
        "name": "ABC Bank Checking",
        "created": "2014-09-04T23:19:19.543Z"
      }
    ]
  }
}
```

```ruby
account_url = 'https://api.dwolla.com/accounts/4bb512e4-ad4d-4f7e-bfd0-a232007f21a1'

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
funding_sources = app_token.get "#{account_url}/funding-sources"
funding_sources._embedded['funding-sources'][0].name # => "ABC Bank Checking"
```

```javascript
var accountUrl = 'https://api-sandbox.dwolla.com/accounts/4bb512e4-ad4d-4f7e-bfd0-a232007f21a1';

appToken
  .get(`${accountUrl}/funding-sources`)
  .then(function(res) {
    res.body._embedded['funding-sources'][0].name; // => 'ABC Bank Checking'
  });
```

```python
account_url = 'https://api.dwolla.com/accounts/4bb512e4-ad4d-4f7e-bfd0-a232007f21a1'

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
funding_sources = app_token.get('%s/funding-sources' % account_url)
funding_sources.body['_embedded']['funding-sources'][0]['name'] # => 'ABC Bank Checking'

```

```php
<?php
$accountUrl = 'https://api.dwolla.com/accounts/4BB512E4-AD4D-4F7E-BFD0-A232007F21A1';

$fsApi = new DwollaSwagger\FundingsourcesApi($apiClient);

$fundingSources = $fsApi->getAccountFundingSources($accountUrl);
# Access desired information in response object fields
print($fundingSources->_embedded) # => PHP associative array of _embedded contents in schema
?>
```

<nav class="pager-nav">
    <a href="/guides/send-money/onboarding.html">Back to: Customer onboarding</a>
    <a href="create-transfer.html">Next step: Create a transfer</a>
</nav>
