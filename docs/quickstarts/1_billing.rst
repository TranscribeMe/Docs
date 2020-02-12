Billing Information
========
Here you may check your billing information.

**Address:**
To check your billing address:

``GET https://rest-api.transcribeme.com/api/v1/billing/address``

To update your billing address:

``POST https://rest-api.transcribeme.com/api/v1/billing/address``

**REQUEST**::

  {
      "firstName": "{firstName}",
      "lastName": "{lastName}",
      "email": "{email}",
      "address1": "{address1}",
      "address2": null,
      "country": {
          "id": "{country}",
          "name": "{countryName}"
      },
      "state": {
          "id": "{stateId}",
          "name": "{stateName}"
      },
      "city": "{city}",
      "zip": "{zip}"
  }

**Payment information:**
To check your credit card:
``GET https://rest-api.transcribeme.com/api/v1/billing/card``

To update your credit card information or to add different credit cards for integration from your customers, BrainTree API/SDK's must be used to securely collect payment information. Please review the documentation `here <https://developers.braintreepayments.com/start/overview>`_. 

Using the BrainTree setup above, use the following to get a client token:
``GET https://rest-api.transcribeme.com/api/v1/billing/gateway/client-token``

To send the payment method nonce to your server:
``POST https://rest-api.transcribeme.com/api/v1/billing/card``

**REQUEST**::

  {
    "token": "{gatewayToken}"
  }
