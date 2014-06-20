---
groups: ['resources']
groups_weight: 2010
linkTitle: 'Profiles'
title: 'Profiles'
---

A profile is a display of personal data associated with a specific Mendeley user.  In the Mendeley context this profile information contains some core information about a person (e.g. name, email address) along with their professional and academic experience. 

#### Anatomy of a profile ####

Profile objects have the following fields: 

- __id (string)__: Identifier (UUID) of the profile. This is set by the server on creation of the profile and it cannot be modified.
- __first_name (string)__: First name of the user. This is a required field. 
- __last_name (string)__: Last name of the user. This is a required field. 
- __location (object)__: Current location of the user. The location object has the following information: name, latitude, longitude. 
- __display_name (string)__: Name of the user including their title. 
- __email (string)__: Primary email of the user. This is used as username for login into Mendeley services. The email will only be exposed through the GET /profiles/me request, but it will never be exposed to other users. 
- __research_interests (string)__: Comma separated list of research topics the user is interested in. 
- __education (array[Education])__: Collection of education objects, which include the following information: degree, institution, start_date, end_date and website.
- __employment (array[Employment])__: Collection of employment objects, which include the following information: position, institution, start_date, end_date, website, classes and main_employment.
- __academic_status (string)__: Current academic status of the profile. Possible values can be found by doing GET /academic_statuses.
- __link (string)__: URL to the Mendeley public profile page. 
- __discipline (Discipline)__: Current research discipline. Possible values can be found by doing GET /disciplines.
- __photo(object)__: The photo object contains the URL to the profile's picture. There are three sizes available: original, standard and square. 
- __verified (boolean)__: Whether the profile has verified their email address or not. 
- __created_at (string)__: Date when the profile was created. This date is set by the server on creation and is represented in ISO 8601 format. 

#### Getting started with the profiles API ####

1. __Retrieving profile information__

	The Mendeley API provides a few ways to retrive information about a profile. An easy way to retrieve information about the authenticated user is by doing:
	
	    GET /profiles/me

    You can also retrieve information about a profile if you know their id:

        GET /profiles/c0925ecf-4de0-3b61-ae9a-ebe5f7406327

    In both cases, if the request is successful you would get something like:

        {
		  "id": "c0925ecf-4de0-3b61-ae9a-ebe5f7406327",
		  "first_name": "Victor",
		  "last_name": "Henning",
		  "display_name": "Dr. Victor Henning",
		  "link": "http://www.mendeley.com/profiles/victor-henning/",
		  "research_interests": "Emotions, Decision Making, Theory of Reasoned Action, Intertemporal Choice, Motion Picture Economics",
		  "academic_status": "Post Doc",
		  "discipline": {
		    "name": "Psychology",
		    "subdisciplines": [
		      "Cognition"
		    ]
		  },
		  "photo": {
		    "original": "http://s3.amazonaws.com/mendeley-photos/34/de/34de3e865f547ec537cd4fa0435f9c1ba0dbd378.png",
		    "standard": "http://s3.amazonaws.com/mendeley-photos/34/de/34de3e865f547ec537cd4fa0435f9c1ba0dbd378-standard.jpg",
		    "square": "http://s3.amazonaws.com/mendeley-photos/34/de/34de3e865f547ec537cd4fa0435f9c1ba0dbd378-square.jpg"
		  },
		  "verified": true,
		  "location": {
		    "latitude": 52.37,
		    "longitude": 4.89,
		    "name": "Amsterdam, Netherlands"
		  },
		  "created_at": "2008-03-25T16:51:10.000Z",
		  "education": [
		    {
		      "institution": "Wissenschaftliche Hochschule für Unternehmensführung - Otto Beisheim Graduate School of Management",
		      "degree": "Dipl.-Kfm. (MBA)",
		      "start_date": "2000-10-01",
		      "end_date": "2004-08-01",
		      "website": "http://www.whu.edu"
		    },
		    {
		      "institution": "Université Libre de Bruxelles - Free University of Brussel",
		      "degree": "",
		      "start_date": "2002-01-01",
		      "end_date": "2002-06-01",
		      "website": "http://www.ulb.ac.be"
		    },
		    {
		      "institution": "Handelshøyskolen BI - Norwegian School of Management BI",
		      "degree": "",
		      "start_date": "2002-08-01",
		      "end_date": "2002-12-01",
		      "website": "http://www.bi.no"
		    }
		  ],
		  "employment": [
		    {
		      "institution": "Mendeley Ltd.",
		      "position": "Co-Founder & CEO",
		      "start_date": "2007-11-01",
		      "website": "www.mendeley.com"
		    },
		    {
		      "institution": "Bauhaus-University of Weimar",
		      "position": "Lecturer / Doctoral Student",
		      "start_date": "2004-10-01",
		      "end_date": "2010-12-01",
		      "website": "www.uni-weimar.de",
		      "classes": [
		        "The Kid Stays In The Picture: Economics of the Film Industry",
		        "Guru*Lab: Production and Distribution of a Major German Motion Picture",
		        "Guru*Talk: The Film Industry in the 21st Century",
		        "Hedonic Consumption"
		      ]
		    },
		    {
		      "institution": "Oliver Wyman Consulting",
		      "position": "Summer Associate",
		      "start_date": "2003-05-01",
		      "end_date": "2003-08-01",
		      "website": "www.oliverwyman.de"
		    },
		    {
		      "institution": "Revelation Records",
		      "position": "Product Management / PR Dept.",
		      "start_date": "2001-05-01",
		      "end_date": "2001-08-01",
		      "website": "www.revelation-records.com"
		    },
		    {
		      "institution": "Helkon Media AG",
		      "position": "Film Production / Business Affairs Dept.",
		      "start_date": "2002-06-01",
		      "end_date": "2002-08-01",
		      "website": "http://www.imdb.com/company/co0069136/"
		    },
		    {
		      "institution": "Sony Music Entertainment/Columbia Records",
		      "position": "Junior A&R, Talent Scout",
		      "start_date": "2000-01-01",
		      "end_date": "2000-10-01",
		      "website": "www.sonymusic.de"
		    },
		    {
		      "institution": "Elsevier",
		      "position": "Co-Founder/CEO, Mendeley & VP Strategy",
		      "start_date": "2013-04-01",
		      "is_main_employment": true
		    }
		  ]
		}
	
