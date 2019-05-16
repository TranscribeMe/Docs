Workflow Sample
========
The most common use case contains 6 steps: 
.. overview_step3::
1. File Upload 
----------

You can choose your preferred way to deliver recordings:

By uploading content using POST request [Method Documentation]

.. code-block:: html
<h1>POST https://rest-api.transcribeme.com/api/v1/recordings/upload
REQUEST 
Cache-Control: no-cache
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary1234567abcdefg
------WebKitFormBoundary1234567abcdefg
Content-Disposition: form-data; name="name"; filename="FILEPATH/MYFILE.mp3"
Content-Type: audio/mp3
------WebKitFormBoundary1234567abcdefg--
</h1>

By specifying publicly available url [Method Documentation]

Note: If you choose upload via publicly available url, you will need to add additional logic on your side to check the status of recording. 

NOTE: It is not possible to order a recording which is not uploaded to our system.

.. overview_step4::
4. Create Order
----------

After an audio file has been successfully uploaded you are able to order a transcript.

On this step you will send a list of recording id's that will be in the order. 

[Method Documentation]

.. overview_step5::
5. Place Order
----------

After that you will receive an email with instructions to pay if you have not enabled the automatic payment feature or used a promo code with discount. You can also use a promo code created by the TranscribeMe Sales Team to bypass the credit card payment step and instead be billed by invoice. 

[Method Documentation] 

In case if payment by credit cards is required for integration, BrainTree API/SDK's must be used to securely collect payment information from your customers: https://developers.braintreepayments.com/start/overview. 

To get a client token make a get request to /billing/gateway/client-token [Method Documentation] 

To send the payment method nonce to your server make post request to /billing/card [Method Documentation] 

To set billing address make a post request to /billing/address [Method Documentation]

.. overview_step6::
6. Get Results
----------

You will receive transcription results within the agreed TAT. These are available in different formats. 
[Method Documentation]






 
3. Create a new order, passing either a RecordingID or array of RecordingIDs.
POST https://rest-api.transcribeme.com/api/v1/orders
Request object as Content-Type application/json:
REQUEST
{
               "id":"",
               "recordings":["{RecordingID}"]
}
 
**This will return an OrderID.
 
4. Obtain the recordings object from that order.
GET https://rest-api.transcribeme.com/api/v1/orders/{OrderID}
 
5. Update settings within the recording object. It is most common to update type or output here. Those expected values are:
Type - 0: Machine Express. 1: First Draft. 2: Standard. 3: Verbatim
Output - 0: Word. 1: HTML. 2: TXT. 3: PDF. 5: NVivo
 
POST https://rest-api.transcribeme.com/api/v1/orders/{OrderID}/recordings/edit
Request object as Content-Type application/json.
 
Below is a sample recording object as an array, but yours should be obtained using the method in step 2.
REQUEST
  [
        {
            "id": "{RecordingID}",
            "settings": {
                "language": "en",
                "accent": "en-AE",
                "type": 0,
                "domain": 0,
                "output": 0,
                "turnaround": 48,
                "speakers": 5,
                "isNoisyAudio": false,
                "isHeavyAccent": false
            }
        }
    ]
 
6. If you have a promo code to use, apply it here.
POST https://rest-api.transcribeme.com/api/v1/orders/{OrderID}/promocode
Request object as Content-Type application/json.
REQUEST
{
  "code": "YOUR_PROMO_CODE"
}
 
7. Place the order.
POST https://rest-api.transcribeme.com/api/v1/orders/{OrderID}/place
Request object as Content-Type application/json.
**Note the code for billingType below, as it should be passed as an array.
REQUEST
[
  {
    "billingType": 0
  }
]
 
8. To query the status of the order, use the method from step 4. Here is the list of possible statuses:
0: Uploading. 1: Ready to Transcribe. 2: In Progress. 3: Transcribed. 4: Error
 
9. Once the status is 3 (Transcribed), you can view the transcript.
GET https://rest-api.transcribeme.com/api/v1/recordings/{RecordingID}/text
