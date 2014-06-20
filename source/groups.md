---
groups: ['resources']
groups_weight: 2010
linkTitle: 'Groups'
title: 'Groups'
---

Groups is the easiest way to collaborate with other researchers in Mendeley. 

There are three types of groups in Mendeley with different sharing and privacy levels:

#### Anatomy of a group

A group object has the following fields:

- __id(string)__: Identifier (UUID) of the group. This identifier is set by the server on create and cannot be modified.
- __link(string)__: URL of the group page in Mendeley.
- __owning_profile_id(string)__: Profile identifier of the owner of the group.
- __access_level(string)__: Whether the group is public, private or invite_only. More on access levels below. 
- __created(string)__: Date the group was created. This date is set by the server after a successful create reques and is represented in ISO 8601 format.
- __name(string)__: The name of the group.
- __description(string)__: Small description of what the group is about. 
- __photo(object)__: The photo object contains the URL to the photo/logo of the group. There are three sizes available: original, standard and square. 
- __webpage(string)__: Website of the group outside Mendeley.
- __disciplines(array)__: List of disciplines the group is part of.
- __tags(array)__: List of tags assigned to the group.
- __role(string)__: Role of the authenticated user in the group. More on group roles below. 

#### Group roles

- __Owner__: the creator of the group. 
- __Admin__: administrator of the group. Only owners and admins can make other members administrators. 
- __User__: normal users of the group. Users can add new references, add files and start discussions in the newsfeed of the group.  
- __Follower__: followers of the group. Only public groups can have followers. Followers cannot interact with other members, i.e. post in newsfeed and participate in discussions, nor add references to the group. This is a read-only access membership.


#### Group access levels

- __Private__: These groups are great for sharing references and full-text articles. Only members of the group are able to access the group and download the shared files.
- __Invite-only (public)__: These groups are publicly accessible through Mendeley search but it requires an invitation to join them. It is only possible to share references. Any files added to the group won't be available for download to any members. Good for sharing references or reading lists.
- __Open (public)__: These groups are publicly accessible through the Mendeley search and any Mendeley user can join them. It is only possible to share references. Good for crowd sourcing reading lists.

#### Getting started with the Groups API

1. __Retrieving groups__

	You can retrieve all groups that the user belong to by performing the following request:
	
	    GET /groups 
    
    A successful response will return a 200 OK code status and a collection of group objects.

    You can also retrieve information for a single group by passing its id in the request:

        GET /groups/247f3e38-8b92-3692-bba3-d922b7805126

    Response example: 

        {
		  "name": "Test group",
		  "description": "This is a group for testing the Mendeley API",
		  "photo": {
		    "standard": "http://s3.amazonaws.com/mendeley-photos/awaiting.png",
		    "square": "http://s3.amazonaws.com/mendeley-photos/disciplines/small/computer-and-information-science.png"
		  },
		  "webpage": "",
		  "disciplines": [
		    6
		  ],
		  "tags": [],
		  "id": "247f3e38-8b92-3692-bba3-d922b7805126",
		  "created": "2014-06-02T12:08:41.000Z",
		  "owning_profile_id": "e2cc65ad-5ef2-3838-b934-737fdb3b196d",
		  "link": "http://www.mendeley.com/groups/4542291/test-group/",
		  "access_level": "private"
		}	

2. __Retrieving members information__

    You can see the members in a group by perfoming the following request: 
        
        GET /groups/247f3e38-8b92-3692-bba3-d922b7805126/members

    A successful request will return a 200 OK status code and a collection of objects with the fields:
    - __profile_id(string)__: UUID of the member's profile. 
    - __role(string)__: What type of role the member has in the group.
    - __joined(string)__: Date that the user joined the group. This date is set by the server and is represented in ISO 8601 format. 

         



