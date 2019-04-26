.. PublicAPI documentation master file, created by
   sphinx-quickstart on Tue Apr  2 16:10:03 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Introduction
=====================================
To use the TranscribeMe API, you must request an API key to use in all API requests. 
You can request an API key on this form. 
The API key should be passed in the API calls as a custom header called "X-Api-Key". 
Authentication
API authentication is achieved via a bearer token which identifies a single user. 
Access token should be passed in the API calls as an authorization header parameter called "Bearer" (like 'Bearer [YOUR TOKEN]'). 
In order to get an access token some additional data must be sent in the request:

client_id (X-Api-Key) - this is provided by TranscribeMe
client_secret - this is provided by TranscribeMe
username - Username (email) of the portal account
password or applicationtoken
For the first-time login uder particular account you should use password and grant_type=password.
Request:
POST /api/v1/token
BODY: grant_type=password&client_id=[X-Api-Key]&client_secret=[CLIENT APP SECRET]&username=[USER NAME]&password=[PASSWORD]
Response:
application/json, text/json
                            {
                "access_token":"[YOUR TOKEN]",
                "token_type":"bearer",
                "expires_in":35999,
                "refresh_token":"[YOUR REFRESH TOKEN]",
                "userName":"[USER EMAIL]",
                "userId":"[USER ID]",
                "firstName":"[USER FIRST NAME]",
                "lastName":"[USER LAST NAME]",
                "roles":"[USER ROLES LIST COMMA SEPARATED]",
                ".issued":"[ISSUE DATE]",
                ".expires":"[EXPIRES DATE]"
            }
        
        
After that you will be able to generate applicationtoken and use it for authentication in the future. 
There are couple of reasons why applicationtoken usage is more preferable than password for API Integration:
Password can be changed on UI and as a result all api calls authorization will fail
Passwords have policies and it is required to change password periodically
Applicationtoken can be regenerated using regenerate method. 
Here is the sample of getting Access token using applicationtoken:
Request:
POST /api/v1/token
BODY: grant_type=applicationtoken&client_id=[X-Api-Key]&client_secret=[CLIENT APP SECRET]&username=[USER NAME]&authtoken=[AUTHTOKEN]
Response:
application/json, text/json
                            {
                "access_token":"[YOUR TOKEN]",
                "token_type":"bearer",
                "expires_in":35999,
                "refresh_token":"[YOUR REFRESH TOKEN]",
                "userName":"[USER EMAIL]",
                "userId":"[USER ID]",
                "firstName":"[USER FIRST NAME]",
                "lastName":"[USER LAST NAME]",
                "roles":"[USER ROLES LIST COMMA SEPARATED]",
                ".issued":"[ISSUE DATE]",
                ".expires":"[EXPIRES DATE]"
            }
        
        
The access token lifetime is 1 hour. You can use the refresh_token for getting a new access token when the old one is expired. You just need to make the following POST request:

POST /api/v1/token
BODY: grant_type=refresh_token&refresh_token=[REFRESH TOKEN]&client_id=[CLIENT APP ID]&client_secret=[CLIENT APP SECRET]
Our API also supports oAuth2. If you're going to obtain a bearer token using an external token the POST request is as follows:

POST /api/v1/token
BODY: grant_type=externaltoken&authtoken=[EXTERNAL TOKEN]&provider=[PROVIDER NAME]&role=[USER ROLE]client_id=[CLIENT APP ID]&client_secret=[CLIENT APP SECRET]
For now, the following providers are supported:

Facebook
Google
Important: The external auth token should allow access to user profile information, including email.
Error Details
The API uses two different formats to describe an error.

Authentication error object
When the application makes requests to the API related to authentication or authorization (e.g. retrieving an access token or refreshing an access token) the error response follows RFC 6749 on The OAuth 2.0 Authorization Framework. Below is an example of a failing request to refresh an access token.

application/json, text/json
                    {
                    "error": "invalid_client",
                    "error_description": "Invalid client secret"
                    }
                
Regular error object
Apart from the response code, unsuccessful responses return information about the error as an error JSON object containing the StatusCode and the array of error messages. Here is an example error response:

application/json, text/json
                    {
                    StatusCode: 400, 
                    Messages: ["Some error message goes here", "Another error message goes here"]
                    }
                    

.. toctree::
   :maxdepth: 4
   :hidden:
   :caption: Quickstarts
   
   quickstarts/0_overview
   quickstarts/1_billing
   quickstarts/1_request_token



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
