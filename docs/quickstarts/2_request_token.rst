Authentication
========
To use the TranscribeMe API, you must request an API key to use in all API requests. You can request an API key `on this form <https://transcribeme.wufoo.com/forms/z88657713u58wc/>`. The API key should be passed in the API calls as a custom header called "X-Api-Key", which is also called the "client_id".

**Authentication**
API authentication is achieved via a bearer token which identifies a single user. 
Access token should be passed in the API calls as an authorization header parameter called "Bearer", which is typically used like 'Bearer {YOUR TOKEN}'. 

In order to get an access token some additional data must be sent in the request:

1. client_id (X-Api-Key) - this is provided by TranscribeMe
2. client_secret - this is provided by TranscribeMe
3. username - Username (email) of the portal account
4. password or applicationtoken

For the first-time login uder particular account you should use password and **grant_type=password** with the below method:

``POST https://rest-api.transcribeme.com/api/v1/token``

**REQUEST**:: 

  grant_type=password&client_id={X-Api-Key}&client_secret={CLIENT APP SECRET}
  &username={USER NAME}&password={PASSWORD}

**RESPONSE** *(Content-type: application/json)*::

  {
    "access_token":"{YOUR TOKEN}",
    "token_type":"bearer",
    "expires_in":35999,
    "refresh_token":"{YOUR REFRESH TOKEN}",
    "userName":"{USER EMAIL}",
    "userId":"{USER ID}",
    "firstName":"{USER FIRST NAME}",
    "lastName":"{USER LAST NAME}",
    "roles":"{USER ROLES LIST COMMA SEPARATED}",
    ".issued":"{ISSUE DATE}",
    ".expires":"{EXPIRES DATE}"
  }
        
        
Then you will be able to generate {access_token} and use **grant_type=applicationtoken** for authentication in the future. 
There are couple of reasons why applicationtoken usage is more preferable than password for API Integration:
1. Password can be changed on UI and as a result all api calls authorization will fail
2. Passwords have policies and it is required to change password periodically
3. Applicationtoken can be regenerated using regenerate method. 

Here is the sample of getting Access token using applicationtoken:
``POST https://rest-api.transcribeme.com/api/v1/token``

**REQUEST**::

  grant_type=applicationtoken&client_id={X-Api-Key}&client_secret={client_secret}
  &username={username}&authtoken={access_token}

**RESPONSE** is similar to the one shown in the grant_type=password example above.        
        
The access_token lifetime is 1 hour. You can use **grant_type=refresh_token** for getting a new access token when the old one is expired. You just need to make the following POST request:
``POST https://rest-api.transcribeme.com/api/v1/token``

**REQUEST**::
  
  grant_type=refresh_token&refresh_token={refresh_token}
  &client_id={X-Api-Key}&client_secret={client_secret}

Our API also supports oAuth2. If you're going to obtain a bearer token using an external token the POST request is as follows:

``POST https://rest-api.transcribeme.com/api/v1/token``

**REQUEST**::

  grant_type=externaltoken&authtoken=[EXTERNAL TOKEN]&provider=[PROVIDER NAME]
  &role=[USER ROLE]&client_id={X-Api-Key}&client_secret={client_secret}

For now, the Facebook and Google are the only supported providers. 
*Important: The external auth token should allow access to user profile information, including email.*

**Error Details**

The API uses two different formats to describe an error.

1. **Authentication error object**
When the application makes requests to the API related to authentication or authorization (e.g. retrieving an access token or refreshing an access token) the error response follows RFC 6749 on The OAuth 2.0 Authorization Framework. Below is an example of a failing request to refresh an access token.

::

  {
    "error": "invalid_client",
    "error_description": "Invalid client secret"
  }
                
2. **Regular error object**
Apart from the response code, unsuccessful responses return information about the error as an error JSON object containing the StatusCode and the array of error messages. Here is an example error response:

::

  {
    StatusCode: 400,
    Messages: ["Some error message goes here", "Another error message goes here"]
  } 
