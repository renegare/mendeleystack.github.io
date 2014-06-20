---
groups: ['resources']
groups_weight: 2010
linkTitle: 'Folders'
title: 'Folders'
---

Folders are an easy way to organise your documents.

You can have folders in your library and also in your groups. It is also possible to create subfolders. 

Getting started with the folders API

1. __Retrieving folders__

	To retrieve folders we'll perform a GET request to the /folders endpoint:

	    GET /folders
	
	If no parameters are passed in the request, the server will return a collection of all the visible folders for the authenticated user, including both library and group folders.
	
	This request also accepts the following parameters: 

	- __group_id(string)__: Returns all folders in a particular group.
	- __profile_id(string)__: Returns all folders created by the user with this profile id.

2. __Creating a folder__

	To create a folder we'll do a POST request to the /folders endpoint. The body of the request should be a JSON object with the following fields:

	- __name(string)__: The name of the folder. This is a required field.
	- __parent(string)__: If what we are creating is a subfolder, then we need to pass the id of the parent folder here. This is an optional field.
	- __group_id(string)__: If what we want to create is a group folder, we need to provide the id of the group. This is an optional field.
	
	A successful request will return a 201 CREATED object and the newly created folder will be returned in the body. 
	
	Things to have in mind when creating folders:

	- Once you have created a folder you cannot change its nature, i.e if it is a group folder, it will always remain a group folder.
	- When creating subfolders in a group folder, you need to pass the group id as well in the request. Otherwise you will get a 400 BAD REQUEST error.
	- Cycle dependencies are not permitted, i.e if A is parent of B, B cannot be parent of A.

3. __Updating a folder__

	We can only update a folder's name and parent attributes. We'll do this with the PATCH method.

	    PATCH /folders/{id}
	
	Same restrictions as in creating folders apply here. A successful request will result in a 204 NO CONTENT.

4. __Deleting a folder__

	To delete a folder, we can simply do:

	    DELETE /folders/{id}
	
	Where {id} is the id of the folder to be deleted. A successful request will return a 204 NO CONTENT. 
	Please be aware that performing this request will permanently delete the folder and all subfolders in it. Documents will still be available from the library or the group if it was a group folder.

5. __Retrieving documents in a folder__

	You can find out what documents are contained in a folder by doing:
	
	    GET /folders/{id}/documents
	
	Where {id} is the identifier of the folder. 
	
	This endpoint will return a list of ids of the documents in the folder. To retrieve the information about documents, please refer to the documents API.

6. __Adding a document to a folder__

	You can easily add a document to your folder by doing the following request:
	
	    POST /folders/{id}/documents
	
	With the body:
	
	    {
	        "document": "{document_id}"
	    }
	
	Where {document_id} is the UUID of the document that you want to add to the folder with UUID {id}.
	
7. __Deleteing a document from a folder__

	To delete a document from a folder you just need to do a DELETE request to the /folders/{id}/documents/{document_id} endpoint. 
	
	This will remove the relationship between the folder and the document, but you can still access the document in your library or group.
	To remove the document permanently or move it to the trash, please refer to the documents API.

