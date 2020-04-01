.. PublicAPI documentation master file, created by
   sphinx-quickstart on Tue Apr  2 16:10:03 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

---------------------------------------

**Introduction**

Once you `sign up here <https://transcribeme.wufoo.com/forms/z88657713u58wc/>`_, you will receive your own, unique API key and client secret. The key:

1. Uniquely identifies you.
2.  Allows you to generate a token that will give you access to upload recordings, create transcription orders and retrieve output.
3. Should be kept private and should not be shared.
4. The API key should be passed in the API calls as a custom header called "X-Api-Key". 

---------------------------------------

**Authentication**

The API key along with the client secret will be used to generate an initial token.
API authentication is achieved via a bearer token which identifies a single user. 
Click :doc:`quickstarts/2_request_token` to learn more about generating a token for authentication.

---------------------------------------

**Workflow**

Click on our :doc:`quickstarts/0_overview` for instructions on how to upload recordings, create orders, retrieve output and update settings. This will give a basic overview of how to use our API.

---------------------------------------

**Billing Information**

Click :doc:`quickstarts/1_billing` to view past orders and update your billing information.


.. toctree::
   :maxdepth: 4
   :hidden:
   :caption: Quickstarts
   
   quickstarts/2_request_token
   quickstarts/0_overview
   quickstarts/1_billing



TranscribeMe REST API
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
