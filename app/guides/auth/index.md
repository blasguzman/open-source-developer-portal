---
layout: twoColumn
section: guides
type: guide
guide:
    name: auth
    step: overview
title: Dwolla OAuth 2.0
description: Obtain an OAuth access token, used to access data in the Dwolla API on behalf of a user or application.
---

# Dwolla OAuth 2.0

Dwolla utilizes the [OAuth 2 protocol](https://oauth.net/2/) to facilitate authorization. OAuth is an authorization framework that enables a third-party application to obtain access to protected resources (Transfers, Funding Sources, etc.) in the Dwolla API. Access to the Dwolla API can be granted to an application either on behalf of a user or on behalf of the application itself. The following guide will walk through Dwolla's implementation of OAuth 2 and the various flows that can be leveraged by your application depending on your use case.

#### OAuth terminology:

* **Resource Server (Dwolla):** The Dwolla server hosting protected resources (Transfers, Funding Sources, etc.) and responding to requests from an authorized application.
* **Authorization Server (Dwolla):** The Dwolla server issuing access tokens to an authorized application.
* **Client (application):** The application making requests to access protected resources after it has obtained authorization.
* **Resource Owner (user/application):** A user with an existing Dwolla account who grants permission to an application to act on their behalf or an application acting on its own behalf.

## Creating an application
Before you can get started with making OAuth requests, you’ll need to first register an application with Dwolla by logging in and navigating to the applications page. Once an application is registered you will obtain your `client_id` and `client_secret` (aka client credentials), which will be used to identify your application when calling the Dwolla API. The Sandbox environment provides you with a created application once you have signed up for an account. Learn more in our [getting started guide](https://developers.dwolla.com/guides/sandbox-setup/). **Remember:** Your client_secret should be kept a secret! Be sure to store your client credentials securely.

## Dwolla's authorization flow
The OAuth 2 protocol defines four main authorization grant types, more commonly referred to as OAuth flows.

**Application authorization:** - Using the [client credentials grant flow](/guides/auth/client-credentials-flow.html), your application will obtain authorization to interact with the API on its own behalf. This is a server-to-server flow with interaction between an application and the Dwolla API; also known as 2-legged OAuth.

<nav class="pager-nav">
    <a href="" style="display:none;"></a>
    <a href="client-credentials-flow.html">View: Client credentials flow</a>
</nav>
