---
groups: ['resources']
groups_weight: 2010
linkTitle: 'Files'
title: 'Files'
---

Files are the actual file attachments that are associated with documents. A document can have many files associated with it. 
Most often these files are .PDF files, which can be read in the Mendeley applications. Though others extensions are allowed: .doc, .txt, etc.

#### How to use the Files API

The Files API is very simple to use. There are however a few important things to understand:

When files are uploaded to the system, we calculate their sha1. This is a unique identifier of the file. We do not support file versioning at the moment, since when the file content changes, so will its filehash, making it an entire new file in the system. This may change in the future.


1. __Retrieving files information__

    You can retrieve a list of your files by simply doing a GET request to the /files endpoint. 
        
        GET /files

    Responses will contain the following information:

    - __id(string)__: The unique identifier for the file (required for other operations).
    - __file_name(string)__: The name of the file. This is currently a generated name from the metadata of the document that the file is attached to. However, we will support original file names from the upload soon.
    - __mime_type(string)__: The mime type of the file. This is used to work out the extension of the file. More info on mime types [here] (http://en.wikipedia.org/wiki/Internet_media_type).
    - __file_hash(string)__: SHA1 hash of the file. This can be used to check the integrity of the file. 
    - __document_id(string)__: The id of the document the file is attached to.

    There are a few parameters that we can pass in this request that might help narrow down the files you are after:

    - __document_id__: When this is provided, the response will contain only the files attached to that particular document.
    - __group_id__: When this is provided, the response will contain only the files attached to documents that belong to that particular group. 
    - __added_since__: Response will only contain those files uploaded since the 'added_since' date. This date should be an ISO 8601 timestamp.
    - __deleted_since__: Response will only contain those files deleted since this date. It should be an ISO 8601 timestamp.

    __NOTE__: *document_id* and *group_id* are mutually exclusive. Providing both at the same time might end in no results being returned by the API.

    __Examples__

    Getting files added since the 1st of September 2009:

        GET /files?added_since=2009-09-01T16%3A39%3A41.000Z

    Get all the files associated with document bfde49d5-732c-326f-8c8e-c851286e00a0:

        GET /files?document_id=bfde49d5-732c-326f-8c8e-c851286e00a0

2. __Downloading a file__

    To download a file, you only need to provide its id as returned on the previous call.
    
        GET /files/{id}

    On a successful request the server will respond with a 303 SEE OTHER response together with a Location header. The URL in this location header is signed for a limited period of time where the file can be downloaded. 

3. __Uploading a file__

    To upload a file through the API we require the use of headers to provide information needed to the server that cannot be passed in the body (since the binary information needs to be transmitted there).
    The following headers are required by the files API:

    - __Content-Type__: This header will tell the server the file's media type. For example, application/pdf.
    - __Content-Disposition__: This header is used to extract the file's name. The value should be, for example: attachment; filename="example.pdf"
    - __Link__: We use this header to specify the document that the file should be attached to. For example: https://mix.mendeley.com/documents/cfb60e27-d3e4-397d-bb59-fc8388b98152

        Example request:

            POST /files
            Link: https://mix.mendeley.com/documents/cfb60e27-d3e4-397d-bb59-fc8388b98152
            Content-Type: "application/pdf"
            Content-Disposition: attachment; filename="example.pdf"

    A successful request will respond with a 201 CREATED code and a Location header where you can find the URL where you can download the file:

        201 CREATED
        Location: https://mix.mendeley.com/files/7574ef29-e090-5c4c-1903-40018b6c3063

4. __Deleting a file__

    To delete a file you must simply do:
    
        DELETE /files/{id}

    A successful request will respond with a 204 NO CONTENT code. 

    Files are permanently deleted and cannot be recovered. If you wish to recover the file, you must upload it again. 

    __IMPORTANT__: When deleting a file from a group document, be aware that this will affect all members in the group and the file will disappear from their library as well.
