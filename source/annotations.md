---
title: Annotations

language_tabs:
  - shell
  - ruby
  - python

toc_footers:
  - <a href="http://dev.mendeley.com/">Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Annotations

Annotations provide a way to highlight or add comments on a section of a file in order to be able to identify particular pieces of it or share points of view on it.

## Attributes of an Annotation

Parameter | Type | Description
--------- | ------- | ----------- |
id | string | identifier (UUID)
created | string |  date the annotation was created. This date is set by the server after a successful create request. This date is represented in ISO 8601 format.
last_modified | string |  date the annotation was updated. This date is updated by the server after a successful update request. This date is represented in ISO 8601 format.
color | color |  RGB values
text | string |  text value of the annotation
positions | array of HighlightBox's | wrapper object contains page and coordinates of the annotation bounding box (IMPROVE THIS)
privacy_level | string | public (DESCRIBE THESE), group or private  |
document_id | string |  UUID of the document which the file is attached to.
profile_id | string |  UUID of the user that created the annotation.
filehash | string |  filehash of which the annotation belongs to.


##  Retrieving annotations
By default will return a paginated collection of all private annotations in your library.

> Returns JSON structured like this:

```json

{
    "id": "d6526d08-22f2-4c1f-b80b-092e6e3df528",
    "color": {
      "r": 255,
      "g": 255,
      "b": 0
    },
    "text": "This is a sticky note",
    "positions": [
      {
        "top_left": {
          "x": 269.035,
          "y": 695.428
        },
        "bottom_right": {
          "x": 269.035,
          "y": 695.428
        },
        "page": 1
      }
    ],
    "created": "2011-10-07T14:19:09.000Z",
    "last_modified": "2011-10-07T14:19:37.000Z",
    "privacy_level": "private",
    "filehash": "bd3293b2-d20c-0d9f-3773-df51a506c7b2",
    "document_id": "2a3f09be-75b0-3bf9-bdbd-838c5e588969",
    "profile_id": "e2cc65ad-5ef2-3838-b934-737fdb3b196d"
  }

```

### HTTP Request

`GET https://mix.mendeley.com/annotations`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
group_id | string | UUID of the group you would like to get the annotations from. This will only return all the annotations in that group.
document_id | string | UUID of the document you would like to get the annotations from. This will only return the annotations associated with that document.

### Returns
The annotation object or an empty collection if both ```group_id``` and ```document_id``` are set. These are mutually exclusive.



## Creating an annotation
Creates a new annotation.

> Create using JSON structure like this:

```json
{

    "color": {
      "r": 255,
      "g": 255,
      "b": 0
    },
    "text": "This is a sticky note",
    "positions": [
      {
        "top_left": {
          "x": 269.035,
          "y": 695.428
        },
        "bottom_right": {
          "x": 269.035,
          "y": 695.428
        },
        "page": 1
      }
    ],
    "privacy_level": "private",
    "filehash": "bd3293b2-d20c-0d9f-3773-df51a506c7b2",
    "document_id": "2a3f09be-75b0-3bf9-bdbd-838c5e588969",
    "profile_id": "e2cc65ad-5ef2-3838-b934-737fdb3b196d"
  }
```


### HTTP Request

`POST https://mix.mendeley.com/annotations`

Parameter | Type | Description
--------- | ------- | ----------- |
color | color |  RGB values
text | string |  text value of the annotation
positions | array of HighlightBox's | wrapper object contains page and coordinates of the annotation bounding box (IMPROVE THIS)
privacy_level | string | public (DESCRIBE THESE), group or private (default)  |
filehash | string |  filehash of which the annotation belongs to.
document_id | string |  UUID of the document which the file is attached to.


### Returns
Returns the annotation object if it successed.


## Updating an annotation
Updates a specific annotation. Any parameters not provided will be left unchanged. The fields that can be updated are: text, color, positions, privacy_level and filehash. The fields document_id, profile_id and created, are only allowed to be set on creation.

> Update the text and positions of the annotation example

```json
    {
      "color": {
        "g": 0,
        "b": 255,
        "r": 0
      },
     "positions": [
          {
            "top_left": {
              "x": 99.082,
              "y": 718.188
            },
            "bottom_right": {
              "x": 300.127,
              "y": 736.125
            },
            "page": 1
          }
        ],
      "text": "This is an updated annotation"
    }
```

### HTTP Request

`PATCH https://mix.mendeley.com/annotations/{id}`

Parameter | Type | Description
--------- | ------- | ----------- |
color | color |  RGB values
text | string |  text value of the annotation
positions | array of HighlightBox's | wrapper object contains page and coordinates of the annotation bounding box (IMPROVE THIS)
privacy_level | string | public (DESCRIBE THESE), group or private (default)  |
filehash | string |  filehash of which the annotation belongs to.
document_id | string |  UUID of the document which the file is attached to.


### Returns
Returns the annotation object if it successed. (TICKET OUTSTANDING FOR THIS)

##  Deleting an annotation
Deletes an annotation


### HTTP Request

`DELETE https://mix.mendeley.com/annotations/{id}`

### Returns
If the request is successful then the server will return a ```204 NO CONTENT``` response.
