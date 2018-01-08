---
layout: twoColumn
section: guides
type: guide
guide:
    name: sandbox-setup
    step: overview
title:  Get started with integrating bank transfers into your application
description: Test programmatic bank transfers with Dwolla's bank transfer API in our developer sandbox.
---

# Getting started in Sandbox

## Sandbox environment

The Sandbox environment is a complete replica of the Dwolla production environment, supporting all of the same API endpoints. Applications should be tested against the Sandbox environment before being used in production.

### Differences from production environment

- The Sandbox contains only test data and is completely separate from your production account.
- All API endpoints have a base URL of `https://api-sandbox.dwolla.com/` instead of `https://api.dwolla.com/`
- Actual money is not sent or received as part of test transactions. 

<ol class = "alerts">
    <li class="alert icon-alert-info">
        Real financial data should never be used in the Sandbox.
    </li>
</ol>

### Transfer behavior in the Sandbox

Unlike balance sourced transfers, which are processed instantaneously, bank-sourced transfers exist in the pending state for a few business days until they are `processed`, `failed`, or `cancelled`.

The Sandbox environment does not replicate any ACH processes, so a `pending` transfer will not clear or fail automatically after a few business days as it would in production. It will simply remain in the `pending` state indefinitely. Reference our [testing resource article](/resources/testing.html) for more information on how-to simulate bank transfer processing in the Sandbox environment.

## Sandbox setup

To set up your Sandbox account, all we ask for is a valid email address. Once you agree to the Dwolla Developer Terms and Service, you will receive an email asking to verify your email address.
After email verification, your Sandbox account will be created and you'll be redirected to our Sandbox Dashboard at `https://dashboard-sandbox.dwolla.com/`. Here you can view your API key and secret and [generate an OAuth access token](/resources/token-generator.html). Dwolla will also associate a funding source, add $5000 to the account balance, and create an application.

<a href="https://accounts-sandbox.dwolla.com" target="_blank" class="btn secondary large">Create your Sandbox account</a>

## Next Steps

Once you have a valid Sandbox account, you can start making calls to the API. To simplify development, learn about configuring and using our [SDKs](/pages/sdks.html). You can also jump straight into the [API docs](https://docsv2.dwolla.com/) or continue to our guides for step by step instructions for your appropriate funds flow.

<nav class="grid-nav">
    <a href="/guides/send-money" class="icon-guides-send-small grid-nav__item">
        <h3>Send money to your users</h3>
        <p><strong>Funds flows</strong></p>
        <p>-  Pay-ins</p>
        <p>-  Facilitator pay-ins</p>
    </a>
    <a href="/guides/receive-money" class="icon-guides-receive-small grid-nav__item">
        <h3>Receive money from your users</h3>
        <p><strong>Funds flows</strong></p>
        <p>-  Pay-outs</p>
        <p>-  Facilitator pay-outs</p>
    </a>
    <a href="/guides/transfer-money-between-users" class="icon-guides-transfer-small grid-nav__item">
        <h3>Transfer money betwen users</h3>
        <p><strong>Funds flows</strong></p>
        <p>*  Pay-ins</p>
        <p>*  Pay-outs</p>
        <p>*  Facilitator pay-ins</p>
        <p>*  Facilitator pay-outs</p>
    </a>
</nav>