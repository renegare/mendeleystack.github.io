---
groups: ['resources']
groups_weight: 2010
linkTitle: 'Documents'
title: 'Documents'
---

Documents are the heart of the Mendeley ecosystem.

There are different flavors of documents in Mendeley: user and catalog documents. Both of them share the same model or schema, with a few differences between them.

#### Anatomy of a document ####

All documents in Mendeley present the following metadata:

- __id(string)__: Identifier (UUID) of the document. This identifier is set by the server on create and it cannot be modified.
- __type(string)__: The type of the document. Supported types: journal, book, generic, book_section, conference_proceedings, working_paper, report, web_page, thesis, magazine_article, statute, patent, newspaper_article, computer_program, hearing, television_broadcast, encyclopedia_article, case, film, bill.
- __month(integer)__: Month in which the document was issued/published. This should be an integer between 1 and 12.
- __year(integer)__: Year in which the document was issued/published.
- __day(integer)__: Day in which the document was issued/published. This should be an integer between 1 and 31.
- __source(string)__: Publication outlet, i.e. where the document was published.
- __title(string)__: Title of the document. This is a required field. 
- __revision(string)__: Number identifying the item (e.g. a report number).
- __abstract(string)__: Brief summary of the document.
- __authors(array)__: List of authors of the document. This is a collection (array) of person objects, which include surname and forename (strings).
- __pages(string)__: range of pages the document covers in the issue, e.g 4-8.
- __volume(string)__: volume holding the document, e.g "2" from journal volume 2.
- __issue(string)__: issue holding the document, e.g. "5" for a journal article from journal volume 2, issue 5
- __website(string)__: Where the document was accessed from.
- __publisher(string)__ 
- __city(string)__: City where the document was published. 
- __edition(array)__: edition holding the document, e.g. "3" from the third edition of a book. 
- __institution(string)__: Institution in which the document was published.
- __series(string)__: title of the collection holding the document (e.g. the series title for a book)
- __chapter(string)__: Chapter number.
- __editors(array)__: List of editors of the document. Like authors, this is a collection (array) of person objects, which include surname and forename (strings).
- __identifiers(array)__: List of identifiers available for the document. The supported identifiers are: arxiv, doi, isbn, issn, pmid (PubMed), scopus and ssrn. 


Example: 

    {
	    "id": "cfb60e27-d3e4-397d-bb59-fc8388b98152",
	    "title": "Spintronics: Spins take sides",
	    "type": "journal",
	    "authors": [
	      {
	        "forename": "Igor",
	        "surname": "Žutić"
	      }
	    ],
	    "year": 2009,
	    "source": "Nature Physics"
    }

__NOTE__: some fields might be missing from the response from the server. This simply means the field has not been set. 

#### User Documents

User documents are those added by users to their libraries. There's two subcategories in user documents:

* **Library documents**: those added by users to their private library. These documents are only accessible to their owners.
    Start by exploring your [library documents](https://mix.mendeley.com/apidocs#!/documents/getDocuments_get_0).

* **Group documents**: those added by users to a group they belong to. These documents will be accessible to any users in the group.
    You can access group documents by specifying the group id in the group id textbox [in the documents call](https://mix.mendeley.com/apidocs#!/documents/getDocuments_get_0).

User documents provide some extra fields to the bibliographic metadata that help to keep the information in sync among different clients:

- __added(string)__: Date the document was added to the system. This date is set by the server after a successful create request. This date is represented in ISO 8601 format. 
- __last_modified(string)__: Date in which the document was last modified. This date is set by the server after a successful update request. This date is represented in ISO 8601 format.
- __profile_id(string)__: Profile id (UUID) of the Mendeley user that added the document to the system. 
- __group_id(string)__: Group id (UUID) that the document belongs to.
- __read(boolean)__: Flag used to identify whether the document has been read or not.
- __starred(boolean)__: Flag used to identify whether the user has marked the document as favourite.
- __authored(boolean)__: Flag used to identify whether the user has authored the document.
- __confirmed(boolean)__: Flag to identify whether the metadata of the document is correct after it has been extracted from the PDF file.
- __hidden(boolean)__: Flag to identify whether Mendeley can publish this document to the Mendeley catalog. 

##### Getting started with the Documents API

The Documents API provides a way to get your user documents. There are a few endpoints that can be used to retrieve them:

1. __Creating new documents__

    To add or create new documents in your library you can just do a POST request to the /documents endpoint. 
    The following fields are required in new documents: title and type.

    You can get a list of [possible types](https://mix.mendeley.com/apidocs#!/document_types/getAllDocumentTypes) by doing:
   
         GET /document_types

    The response to this request will include an 'id' and a 'name'. You will have to use the id provided in your create request. You can use the name as a display name.

    Example request:

        {
		  "read": false,
		  "starred": false,
		  "authored": false,
		  "confirmed": false,
		  "hidden": false,
		  "type": "journal",
		  "year": 2009,
		  "source": "Molecular Cell",
		  "title": "How To Choose a Good Scientific Problem",
		  "identifiers": {
		      "doi": "10.1016/j.molcel.2009.09.013",
		      "issn": "10972765"
		  },
		  "authors": [
		    {
		      "surname": "Alon",
		      "forename": "Uri"
		    }
		  ],
		  "pages": "726-728",
		  "volume": "35",
		  "issue": "6"
		} 

    After a successful create request, the server will return a 201 CREATED response with the URL of the newly created resource available in the Location header.

    Example response: 

        201 CREATED
        Location: https://mix.mendeley.com/documents/c6b35709-6791-3bac-9476-bb9fe75d7eaf

    The 'added' timestamp will be set automatically by the server, as well as the 'id' and the 'last_modified' timestamp. So, bear in mind, that if you pass these fields
on the request, they will be ignored and overriden by the server.

    The response will also contain a body with the newly created document. Note the server set fields are returned here: 

        {
		  "id": "c6b35709-6791-3bac-9476-bb9fe75d7eaf",
		  "title": "How To Choose a Good Scientific Problem",
		  "type": "journal",
		  "authors": [
		    {
		      "forename": "Uri",
		      "surname": "Alon"
		    }
		  ],
		  "year": 2009,
		  "source": "Molecular Cell",
		  "identifiers": {
		    "doi": "10.1016/j.molcel.2009.09.013",
		    "issn": "10972765"
		  },
		  "added": "2014-05-29T10:43:57.972Z",
		  "pages": "726-728",
		  "volume": "35",
		  "issue": "6",
		  "profile_id": "e2cc65ad-5ef2-3838-b934-737fdb3b196d",
		  "read": false,
		  "starred": false,
		  "authored": false,
		  "confirmed": false,
		  "hidden": false
		}

2. __Retrieving documents__

    To retrieve documents in your library you can start by using the [/documents] (https://mix-staging.mendeley.com/apidocs#!/documents/getDocuments) endpoint.
A request to this endpoint without parameters will return a collection of all your library documents. 
    If group_id is passed in the request, the response will contain a collection of all documents that belong to that particular group. 

    The information returned by the server depends on the 'view' parameter passed on the request. 
If this parameter is not passed in the request the response will include the fields: abstract, type, title, source, year, authors and identifiers. This is what we call the core of the document object.
    There are three possible values for the 'view' paramenter: 
    - __bib__: Will return the core fields plus the bibliographic data. This includes: pages, volume, issue, website, month, publisher, day, city, edition, institution, series, chapter, revision and editors. 
    - __client__: Will return the core fields plus the fields needed for clients to sync across different devices like read, starred, authored, confirmed and hidden.
    - __all__: Will return the core fields, plus the bibliographic fields plus the client fields. 

    The 'id', 'last_modified' and the 'profile_id' values are always returned. The 'group_id' will only be returned for group documents.

    All collections returned in the API are paginated. You can control the size of the pages with the 'limit' parameter. You can also sort the collection in ascending or descending order by a certain field with the 'sort' and 'order' parameters. For example, to get your collection of documents with a pages of 30 elements, ordered by title ascending:
    
        GET /documents?limit=30&sort=title&order=asc

    You will receive links to navigate the pages in Link headers in your response if they are available. The Link header provides a 'rel' attribute that can have the following values:

    - __first__: Provides a URL to go to the first page of the collection. This link will be omitted if already in the first page.
    - __last__: Provides a URL to go to the last page of the collection. This link will be omitted if already in the last page.
    - __next__: Provides a URL to go to the next page of the collection. If there is only one more page to navigate to then only the 'last' link header will be present. 
    - __previous__: Provides a URL to go to the previous page of the collection. 

    __IMPORTANT__: Please follow the URLs as they appear in the Link headers to navigate through the pages.  

    2.1. *Retrieving subcollections of documents*

    If you don't want to retrieve all the documents for a particular library or group, you can always retrieve just the subcollection of documents that have been updated since a specific point in time. 
    To do this, you can use the 'modified_since' parameter. This parameter accepts a timestamp in ISO 8601 format.

        GET /documents?modified_since=2011-06-23T23:06:40.000Z

    In a similar fashion, you can also retrieve the documents that have been deleted from your account by using the 'deleted_since' parameter. Again, this parameters accepts a timestamp in ISO 8601 format. Please note though, this call will only return objects ids. 

    Example request, get all deleted documents since 23rd of June 2011:

        GET /documents?deleted_since=2011-06-23T23:06:40.000Z

    Response:

        [
		  {
		    "id": "a5408ce8-5240-3615-91cf-1075cd616d87"
		  }
		]

    2.2. *Retrieving single documents*
   
     You can also retrieve the basic metadata for a single document by doing:
            
        GET /documents/cfb60e27-d3e4-397d-bb59-fc8388b98152

     The id passed in this request is the one set by the server. Alternatively, you can follow the link provided in the Location header in the successful creation request.

     Response example:

        {
		    "id": "cfb60e27-d3e4-397d-bb59-fc8388b98152",
		    "title": "Spintronics: Spins take sides",
		    "type": "journal",
		    "authors": [
		      {
		        "forename": "Igor",
		        "surname": "Žutić"
		      }
		    ],
		    "year": 2009,
		    "source": "Nature Physics",
		    "added": "2009-09-01T16:39:41.000Z",
		    "profile_id": "e2cc65ad-5ef2-3838-b934-737fdb3b196d",
		    "last_modified": "2011-06-23T23:06:40.000Z"
		  }

    Retrieving client metadata as well as basic:
        
        GET /documents/cfb60e27-d3e4-397d-bb59-fc8388b98152?view=client

    Response:

        {
		  "id": "cfb60e27-d3e4-397d-bb59-fc8388b98152",
		  "title": "Spintronics: Spins take sides",
		  "type": "journal",
		  "authors": [
		    {
		      "forename": "Igor",
		      "surname": "Žutić"
		    }
		  ],
		  "year": 2009,
		  "source": "Nature Physics",
		  "added": "2009-09-01T16:39:41.000Z",
		  "profile_id": "e2cc65ad-5ef2-3838-b934-737fdb3b196d",
		  "last_modified": "2011-06-23T23:06:40.000Z",
		  "read": false,
		  "starred": false,
		  "authored": false,
		  "confirmed": true,
		  "hidden": false
		}  

3. __Updating documents__

    To update a document in your library you use the PATCH method to the /documents/{id} endpoint.

    Only the fields you pass in the request will be updated. You can clear a field by passing null values for it, but beware that the same restrictions as for creation apply: both title and type must be present. 

    Example request:

        PATCH /documents/cfb60e27-d3e4-397d-bb59-fc8388b98152
        Body:
        {
		  "read": true,
		  "type": "book",
		  "year": 2014
		}

    A successful request will return 204 NO CONTENT.

4. __Deleting documents and using the Trash__

    There are a few ways of deleting documents through the API. 
    You can either do a soft delete by moving a document to the trash: 

        POST /documents/{id}/trash

    Or, you can do a hard delete by doing the following:

        DELETE /documents/{id}

    When performing a hard delete, the document will be removed forever and it is not possible to restore it.

    However, when doing a soft delete, you can see the documents in the trash:

        GET /trash

    Response: 

        [
		  {
		    "id": "f116690f-5bf4-3756-ba6b-1400ff7509d0",
		    "title": "Identity Provider Deployment",
		    "type": "journal",
		    "authors": [
		      {
		        "forename": "U K",
		        "surname": "Access"
		      },
		      {
		        "forename": "Management",
		        "surname": "Federation"
		      }
		    ],
		    "year": 2008,
		    "added": "2011-07-07T15:44:20.000Z",
		    "profile_id": "e2cc65ad-5ef2-3838-b934-737fdb3b196d",
		    "last_modified": "2014-05-29T11:53:51.000Z"
		  },
		  {
		    "id": "a5408ce8-5240-3615-91cf-1075cd616d87",
		    "title": "About OAuth",
		    "type": "journal",
		    "authors": [
		      {
		        "forename": "Getting",
		        "surname": "Started"
		      }
		    ],
		    "source": "ReVision",
		    "added": "2011-06-23T23:19:01.000Z",
		    "profile_id": "e2cc65ad-5ef2-3838-b934-737fdb3b196d",
		    "last_modified": "2014-05-29T11:53:52.000Z"
		  }
        ]

     And also restore a trashed document by performing:

        POST /trash/{id}/restore

    where {id} is the id of the document to restore.

    You can delete trashed documents definitely by doing a DELETE request to /trash/{id}

    You can also retrieve the list of documents in your trash by doing a GET request to /trash. The response to this request will return a collection of objects that have been moved to the trash. This request also accepts 'view' as a parameter like we saw in the section 'Retrieving documents'.

#### Catalog Documents 

The catalog is Mendeley's crowdsourced collection of papers. Although all documents are public, some of their data may not be accessible due to copyright issues. 

Catalog documents provide the following additional information on top of the base fields:

- __reader_count(integer)__: Total number of users in Mendeley that have the document in their library.
- __reader_count_by_academic_status(object)__: Number of readers based on their academic status.
- __reader_count_by_discipline(object)__: Number of readers based on their discipline.
- __reader_count_by_country(object)__: Number of readers based on their country. 

The GET /catalog endpoint also provides different views just like it was possible for user documents. The possible values are:

- __bib__: Just like in user documents, this view will return the core fields plus the bibliographic data. This includes: pages, volume, issue, website, month, publisher, day, city, edition, institution, series, chapter, revision and editors.
- __stats__: This view will return the core fields plus the statistics data. This includes: reader_count, reader_count_by_academic_status, reader_count_by_discipline and reader_by_country.
- __all__: This view will return the core fields, plus the bib data, plus the stats data. 

The catalog API provides a way to lookup documents based on several identifiers including CrossRef DOIs, publisher issued identifiers (e.g arXiv, PubMed), international standard codes (e.g. ISBN and ISSN) and more. For example,to look up in the catalog a document with DOI 10.1016/j.molcel.2009.09.013 returning the core fields and the stats, you should do:
    
    GET /catalog?doi=10.1016%2Fj.molcel.2009.09.013&view=stats

