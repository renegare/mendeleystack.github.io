---
groups: ['overview']
groups_weight: 2010
linkTitle: 'Response Codes'
title: 'Response Codes'
---

#### Response Codes

The Mendeley API uses standard HTTP codes to provide the clients with information about whether the request has been successful or not. There are two types of codes returned:

##### Successful Codes

- __200 OK__ - The request was successful and there should be information in the body of the response. This response is usually returned after GET requests.

- __201 Created__ - The request was successful; extra information may or may not be returned in the body of the response, for example, after creation we may return the newly created resource.
              This response is usually returned after creation requests.

- __204 No Content__ - The request was successful and no extra information will be provided in the body of the response. This response is usually given after DELETE requests, though it may be returned in other circumstances. 


##### Error Codes

- __400 Bad Request__ - The request you sent to the server is invalid. 

- __401 Unauthorized__ - This error will be returned if the authorization is incorrect - that is, the access_token is not present or it has expired.

- __404 Not Found__ - The resource you were requesting cannot be found. 
