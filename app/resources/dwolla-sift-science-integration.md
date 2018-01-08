---
layout: twoColumn
section: resources
type: integrations
title:  "Sift Science Integration"
description: "As part of Dwolla’s API, Partners can implement Sift Science’s machine learning model for real-time ACH fraud monitoring."
---

# Sift Science Integration

As part of Dwolla’s [API](https://www.dwolla.com/platform), Partners can choose to implement Sift Science’s machine learning model for [real-time ACH fraud monitoring](https://www.dwolla.com/ach/automated-ach-fraud-monitoring).

The need to leverage machine learning in protecting against fraud becomes increasingly valuable as payment volume scales. With this in mind, many Dwolla API Partners look for the right solution to help manage risk for their ACH payments.

Often, we see Partners choosing to use [Sift Science](https://siftscience.com/) in order to better monitor and protect against fraud. We understand the value Sift’s real-time risk scoring model can provide our Partners, so we’ve integrated the service right into the Dwolla API.

In this article we’ll cover the steps to get your Sift Account set up, as well as the process for integrating Sift into your Dwolla API Dashboard.


### Integration overview

* [Register](https://siftscience.com/signup) for a Sift Account
* Visit `Integrations` under your account settings, or visit [https://dashboard.dwolla.com/account/integrations](https://dashboard.dwolla.com/account/integrations)
* Toggle to turn Sift on, and enter your Sift Key

![Animation of Sift Science Integration](/images/sift-integration.gif "Sift Science Integration")

**It is important to note** that your integration with Sift will involve the placement of [Sift Science JavaScript](https://siftscience.com/developers/docs/javascript/javascript-api) within your application. Dwolla is unable to perform this code placement on your behalf, as the Sift Science JavaScript implementation occurs within your application.

As outlined in the bullet points above, first you’ll insert your Sift Science JavaScript within your application. Subsequently, you will copy and paste your Sift Science API key within the Integrations section under `Account` in the Dwolla API Dashboard. **Then you are all set!**

The events below are sent to Sift on your behalf by Dwolla’s API, and there is no additional development needed on your side. Events will begin populating into Sift once your integration is complete.  

| Dwolla Event | Sift Event |
|--------------|------------|
| [Customer Created](https://docsv2.dwolla.com/#create-a-customer) | [$create_account](https://siftscience.com/developers/docs/curl/events-api/reserved-events/create-account) |
| [Customer Updated](https://docsv2.dwolla.com/#update-a-customer) | [$update_account](https://siftscience.com/developers/docs/curl/events-api/reserved-events/update-account) |
| [Customer Suspended](https://docsv2.dwolla.com/#update-a-customer) | [Label User](https://siftscience.com/developers/docs/curl/labels-api/label-user) with $abuse_type = 'account\_abuse' |
| [Customer Funding Source Added](https://docsv2.dwolla.com/#create-a-funding-source-for-a-customer) | [$update_account](https://siftscience.com/developers/docs/curl/events-api/reserved-events/update-account) |
| [Customer Funding Source Removed](https://docsv2.dwolla.com/#remove-a-funding-source) | [$update_account](https://siftscience.com/developers/docs/curl/events-api/reserved-events/update-account) |
| [Transfers](https://docsv2.dwolla.com/#transfers) | [$transaction](https://siftscience.com/developers/docs/curl/events-api/reserved-events/transaction) with $transaction_type = '$transfer' |
| [Transfers In](https://docsv2.dwolla.com/#initiate-a-transfer) | [$transaction](https://siftscience.com/developers/docs/curl/events-api/reserved-events/transaction) with $transaction_type = '$deposit' |
| [Transfers Out](https://docsv2.dwolla.com/#initiate-a-transfer) | [$transaction](https://siftscience.com/developers/docs/curl/events-api/reserved-events/transaction) with $transaction_type = '$withdrawal' |

If you would like to go live with your Dwolla API integration, please [reach out to our sales team](https://www.dwolla.com/contact/). If you already have an Dwolla API Integration and would like to get started with Sift Science, [log in and connect your Sift account](https://dashboard.dwolla.com/account/integrations) now.
