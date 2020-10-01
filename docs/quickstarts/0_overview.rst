Workflow Sample
========
The most common use case contains 5 steps: 

.. overview_step1::
1. File Upload 
----------

You can choose your preferred way to deliver recordings:

- By uploading content:
``POST https://rest-api.transcribeme.com/api/v1/recordings/upload``

**REQUEST**::

     Cache-Control: no-cache
     Content-Type: multipart/form-data; boundary=----WebKitFormBoundary1234567abcdefg
     ------WebKitFormBoundary1234567abcdefg
     Content-Disposition: form-data; name="name"; filename="FILEPATH/MYFILE.mp3"
     Content-Type: audio/mp3
     ------WebKitFormBoundary1234567abcdefg--

----------------

- By specifying publicly available url:
``POST https://rest-api.transcribeme.com/api/v1/recordings/remote``

**REQUEST**::

     {
       "url": "https://www.MYWEBSITE.com/MYPATH"
     }

*If you choose upload via publicly available url, you will need to add additional logic on your side to check the status of recording.*

*It is not possible to order a recording which is not uploaded to our system.*

**This will return a recordingId.**

.. overview_step2::
2. Create Order
----------
After an audio file has been successfully uploaded you are able to order a transcript.
On this step you will send a list of recording id's that will be in the order. 

*(Request object as Content-Type application/json)*
``POST https://rest-api.transcribeme.com/api/v1/orders``

**REQUEST**::

     {
       "id":"",
       "recordings":["{recordingID}"]
     }
 
**This will return an Order json object.**

*You may also obtain the Order object using the following method:*
``GET https://rest-api.transcribeme.com/api/v1/orders/{orderId}``

.. overview_step3::
3. Update settings
----------
Update settings within the recording object. It is most common to update type or output here. Use the endpoints below to obtain these expected values:

Type: ``GET https://rest-api.transcribeme.com/api/v1/transcription/types``

Speakers: ``GET https://rest-api.transcribeme.com/api/v1/transcription/speakers`` 

Output: ``GET https://rest-api.transcribeme.com/api/v1/transcription/outputgroups``

Turnaround: ``GET https://rest-api.transcribeme.com/api/v1/transcription/turnaround``

Language: ``GET https://rest-api.transcribeme.com/api/v1/dictionaries/languages``

Accent: ``GET https://rest-api.transcribeme.com/api/v1/dictionaries/languages/accents?languageId={languageId}``

Domain: ``GET https://rest-api.transcribeme.com/api/v1/transcription/domains``

*(Request object as Content-Type application/json)*
``POST https://rest-api.transcribeme.com/api/v1/orders/{orderId}/recordings/edit`` 

**REQUEST**::

     [
          {
               "id": "{recordingID}",
               "settings": {
                    "language": "{languageId}",
                    "accent": "{accentID}",
                    "type": {type},
                    "domain": {domain},
                    "output": {output},     
                    "turnaround": {turnaround},
                    "speakers": {speakers},
                    "isNoisyAudio": false,
                    "isHeavyAccent": false
               }
          }
     ]
..
     If you need to update currency, you may obtain a list of values here:
     ``GET https://rest-api.transcribeme.com/api/v1/transcription/currencies``
     Then apply the currency here:
     ``POST https://rest-api.transcribeme.com/api/v1/orders/{orderId}/currency``
     **REQUEST**::
          {
               "id": "sample string 1"
          }

Also if you have a promo code to use, you may apply it here:

*(Request object as Content-Type application/json)*
``POST https://rest-api.transcribeme.com/api/v1/orders/{orderID}/promocode``

**REQUEST**::

     {
          "code": "YOUR_PROMO_CODE"
     }

.. overview_step4::
4. Place Order
----------
***IMPORTANT!!!*** If you have been given a promo code to use, you MUST enter it before placing an order. Please see the above step for info about this.

Visit :doc:`1_billing` to confirm that your billing information is setup correctly. You can also use a promo code created by the TranscribeMe Sales Team to bypass the credit card payment step and instead be billed by invoice. 

*(Request object as Content-Type application/json)*
``POST https://rest-api.transcribeme.com/api/v1/orders/{orderID}/place``

**Note the code for billingType below, as it should be passed as an array.**
**REQUEST**::

     [
          {
               "billingType": 0
          }
     ]

To query the status of the order, use the following method:
``GET https://rest-api.transcribeme.com/api/v1/recordings/{recordingId}/status``

For list of available status values use:
``GET https://rest-api.transcribeme.com/api/v1/dictionaries/recordingstatuses``

.. overview_step5::
5. Get Results
----------

You will receive transcription results within the agreed TAT. These are available in different formats. 

To obtain the results as a json object use:
``GET https://rest-api.transcribeme.com/api/v1/recordings/{recordingId}/transcription``

To download the file:
``POST https://rest-api.transcribeme.com/api/v1/recordings/download``

**REQUEST**::

     {
          "recordings": [
               {
                    "id": "{recordingId}",
                    "ownerId": "{userId}"``
               }
          ],
          "output": {output},
          "highlightedOnly": false,
          "removeStrikeout": false
     }

