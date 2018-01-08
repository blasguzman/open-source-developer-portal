---
layout: twoColumn
section: resources
type: integrations
title:  "Dwolla.js"
description: "Quickly integrate instant bank verification for developers using the Dwolla ACH API."
---

# Dwolla.js

Dwolla.js is a client-side JavaScript library with the primary function of securely transmitting sensitive data (bank account and routing number) from your application's front-end to Dwolla without the data passing through your server. When you attach a bank account to a Customer, use dwolla.js and let Dwolla reduce your risk of handling sensitive data. Additionally, Dwolla.js includes an added function available to Dwolla API partners providing the ability to render the instant bank account verification flow within a specified container on the partner's application. However you're using dwolla.js, both server-side and client-side interaction is required.

### Getting Started: Usage and configuration

#### Include dwolla.js
Begin the client-side implementation by including dwolla.js in the HEAD of your HTML page. You can include either the development version(`<script src="https://cdn.dwolla.com/1/dwolla.js"></script>`) or the minified version (`<script src="https://cdn.dwolla.com/1/dwolla.min.js"></script>`) of dwolla.js.

```htmlnoselect
<head>
  <script src="https://cdn.dwolla.com/1/dwolla.js"></script>
</head>
```

#### Configure
Configuration options are available for utilizing dwolla.js in both our Sandbox and production environments. Configuration of an environment should take place after you have included the dwolla.js library.

```javascriptnoselect
// Sandbox
dwolla.configure('sandbox');

// Production
dwolla.configure('prod');
```

* * *

#### View:

*   [Add a bank account](/resources/dwolla-js/add-a-bank-account.html)
*   [Instant account verification](/resources/dwolla-js/instant-account-verification.html)
*   [On-demand bank transfers](/resources/dwolla-js/on-demand-bank-transfers.html)
