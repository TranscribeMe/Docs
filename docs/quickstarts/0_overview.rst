Workflow Sample
========
The most common use case contains 5 steps: 

.. overview_step1::
1. File Upload 
----------

You can choose your preferred way to deliver recordings:

By uploading content using POST request [Method Documentation]

``POST https://rest-api.transcribeme.com/api/v1/recordings/upload
|  REQUEST ``
``|  Cache-Control: no-cache
|  Content-Type: multipart/form-data; boundary=----WebKitFormBoundary1234567abcdefg``
``|  ------WebKitFormBoundary1234567abcdefg
|  Content-Disposition: form-data; name="name"; filename="FILEPATH/MYFILE.mp3"``
``|  Content-Type: audio/mp3
|  ------WebKitFormBoundary1234567abcdefg--``

**Need better example???

By specifying publicly available url [Method Documentation]
``POST https://rest-api.transcribeme.com/api/v1/recordings/remote
| REQUEST {
  "url": "https://www.MYWEBSITE.com/MYPATH"
}
``
*: If you choose upload via publicly available url, you will need to add additional logic on your side to check the status of recording. 

<b>**: It is not possible to order a recording which is not uploaded to our system.</b>   

.. overview_step2::
2. Create Order
----------
After an audio file has been successfully uploaded you are able to order a transcript.
On this step you will send a list of recording id's that will be in the order. 

POST https://rest-api.transcribeme.com/api/v1/orders
Request object as Content-Type application/json:
REQUEST
{
               "id":"",
               "recordings":["{RecordingID}"]
}
 
This will return an Order json object. 

You may also obtain the Order object using the following method:
GET https://rest-api.transcribeme.com/api/v1/orders/{OrderID}

.. overview_step3::
3. Update settings
----------
Update settings within the recording object. It is most common to update type or output here. Those expected values are:
Type - 0: Machine Express. 1: First Draft. 2: Standard. 3: Verbatim
**Is this only available as json??? Output - 0: Word. 1: HTML. 2: TXT. 3: PDF. 5: NVivo
**What about language, accent, turnaround and speakers? 
 
POST https://rest-api.transcribeme.com/api/v1/orders/{OrderID}/recordings/edit
Request object as Content-Type application/json.
 
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

**Currency???

Also if you have a promo code to use, you may apply it here:
POST https://rest-api.transcribeme.com/api/v1/orders/{OrderID}/promocode
Request object as Content-Type application/json.
REQUEST
{
  "code": "YOUR_PROMO_CODE"
}

.. overview_step4::
4. Place Order
----------

After that you will receive an email with instructions to pay if you have not enabled the automatic payment feature or used a promo code with discount. You can also use a promo code created by the TranscribeMe Sales Team to bypass the credit card payment step and instead be billed by invoice. 

POST https://rest-api.transcribeme.com/api/v1/orders/{OrderID}/place
Request object as Content-Type application/json.
**Note the code for billingType below, as it should be passed as an array.
REQUEST
[
  {
    "billingType": 0
  }
]

In case if payment by credit cards is required for integration, BrainTree API/SDK's must be used to securely collect payment information from your customers: https://developers.braintreepayments.com/start/overview. 

To get a client token make a get request to /billing/gateway/client-token [Method Documentation] 

To send the payment method nonce to your server make post request to /billing/card [Method Documentation] 

To set billing address make a post request to /billing/address [Method Documentation]

To query the status of the order, use the following method:
https://rest-api.transcribeme.com/api/v1/recordings/{recordingId}/status
Here is the list of possible statuses:
0: Uploading. 1: Ready to Transcribe. 2: In Progress. 3: Transcribed. 4: Error

.. overview_step5::
5. Get Results
----------

You will receive transcription results within the agreed TAT. These are available in different formats. 
GET https://rest-api.transcribeme.com/api/v1/recordings/{recordingId}/transcription
**How do they retrieve output in different formats, other than json???
