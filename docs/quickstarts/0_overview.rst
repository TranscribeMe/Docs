========
Workflow Sample
========
The most common use case contains 6 steps: 
Workflow sample

----------
.. overview_step1:
1. Registration
----------

To register new user account you will need to make a POST request to /account [Method Documentation].
2. Authentication
Please see authentication explanations here.
3. File Upload 
You can choose your preferred way to deliver recordings:
By uploading content using POST request [Method Documentation]
By specifying publicly available url [Method Documentation]
Note: If you choose upload via publicly available url, you will need to add additional logic on your side to check the status of recording. 
It is not possible to order a recording which is not uploaded to our system.
4. Create Order
After an audio file has been successfully uploaded you are able to order a transcript.
On this step you will send a list of recording id's that will be in the order. 
[Method Documentation]
5. Place Order
After that you will receive an email with instructions to pay if you have not enabled the automatic payment feature or used a promo code with discount. You can also use a promo code created by the TranscribeMe Sales Team to bypass the credit card payment step and instead be billed by invoice. 
[Method Documentation] 

In case if payment by credit cards is required for integration, BrainTree API/SDK's must be used to securely collect payment information from your customers: https://developers.braintreepayments.com/start/overview. 
To get a client token make a get request to /billing/gateway/client-token [Method Documentation] 
To send the payment method nonce to your server make post request to /billing/card [Method Documentation] 
To set billing address make a post request to /billing/address [Method Documentation]
6. Get Results
You will receive transcription results within the agreed TAT. These are available in different formats. 
[Method Documentation]
