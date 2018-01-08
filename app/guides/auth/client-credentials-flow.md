---
layout: twoColumn
section: guides
type: guide
guide:
    name: auth
    step: '2'
title: Dwolla OAuth 2.0 | Client Credentials Flow
description: Obtain an OAuth access token, used to access data in the Dwolla API on behalf of a user or application. Learn more about the client credentials flow.
---

# Overview - Obtaining an application access token

The [client credentials flow](https://tools.ietf.org/html/rfc6749#section-4.1) is used when an application needs to obtain permission to act on its own behalf. An application will exchange it's `client_id`, `client_secret`, and `grant_type=client_credentials` for an [application access token](https://docsv2.dwolla.com/#application-access-token). An application access token can then be used to make calls to the Dwolla API on behalf of the application, for example, when you create a webhook subscription, retrieve events, and list webhooks fired to a subscribed webhook endpoint. The primary reason for obtaining an application access token is for managing webhooks and events. However, Dwolla has modified this grant type by allowing applications to access Dwolla's API [Customer](https://docsv2.dwolla.com/#customers) related endpoints using the application access token if the `ManageCustomers` scope is enabled on the application.

## Request application authorization
The client credentials flow is the simplest OAuth 2 grant, with a server-to-server exchange of your application's `client_id`, `client_secret` for an OAuth application access token. In order to execute this flow, you will need to make an HTTP request from your application server, to the Dwolla authorization server.

##### HTTP request
`POST https://www.dwolla.com/oauth/v2/token`

Including the `Content-Type: application/x-www-form-urlencoded` header, the request is sent to the token endpoint with the following `form-encoded` parameters:

##### Request parameters
| Parameter | Required | Type | Description |
|-----------|----------|----------------|-------------|
| client_id | yes | string | Application key. Navigate to `https://www.dwolla.com/applications` (production) or `https://dashboard-sandbox.dwolla.com/applications` (Sandbox) for your application key |
| client_secret | yes | string | Application secret. Navigate to `https://www.dwolla.com/applications` (production) or `https://dashboard-sandbox.dwolla.com/applications` (Sandbox) for your application secret. |
| grant_type | yes | string | This must be set to `client_credentials`. |

#### Example request

```raw
POST https://sandbox.dwolla.com/oauth/v2/token
Content-Type: application/x-www-form-urlencoded

client_id=CGQXLrlfuOqdUYdTcLz3rBiCZQDRvdWIUPkwasGMuGhkem9Bo&client_secret=g7QLwvO37aN2HoKx1amekWi8a2g7AIuPbD5CcJSLqXIcDOxfTr&grant_type=client_credentials
```

```python
# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python
# This example assumes you've already intialized the client. Reference the SDKs page for more information: https://developers.dwolla.com/pages/sdks.html
application_token = client.Auth.client()
```

```javascript
// Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-node
// This example assumes you've already intialized the client. Reference the SDKs page for more information: https://developers.dwolla.com/pages/sdks.html
client.auth.client()
  .then(function(appToken) {
    return appToken.get('webhook-subscriptions');
  })
  .then(function(res) {
    console.log(JSON.stringify(res.body));
  });
```

```ruby
# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby
# This example assumes you've already intialized the client. Reference the SDKs page for more information: https://developers.dwolla.com/pages/sdks.html
application_token = $dwolla.auths.client
# => #<DwollaV2::Token client=#<DwollaV2::Client id="..." secret="..." environment=:sandbox> access_token="..." expires_in=3600 scope="...">
```

```php
/**
 *  No support for this language yet. We recommend using an external REST client for making OAuth requests.
 **/
```

#### Refreshing an application access token
A refresh token is not paired with an application access token, therefore in order to refresh authorization you'll simply request a new application access token by exchanging your client credentials (as shown above).

That's it! You're ready to start [making requests](/guides/auth/using-an-access-token.html) to the [Dwolla API](https://docsv2.dwolla.com/) on behalf your application.

<nav class="pager-nav">
    <a href="./">Back: Overview</a>
    <a href="using-an-access-token.html">Next step: Using an access token</a>
</nav>
