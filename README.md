Table of Contents
=================

   * [Conventions](#conventions)
   * [Acknowledgments](#acknowledgments)
   * [Resources](#resources)
     * [Applies to all resources](#applies-to-all-resources)
       * [Dates](#dates)
     * [Collection Resources](#collection-resources)
         * [Filtering](#filtering)
         * [Sorting](#sorting)
         * [Pagination](#pagination)
         * [Time selection queries](#time-selection-queries)
         * [Bulk requests](#bulk-requests)
     * [Create resource](#create-resource)
         * [Preventing duplicate creation of resources](#preventing-duplicate-creation-of-resources)
         * [Linking/Unlinking resources together](#linkingunlinking-resources-together)
     * [Read a resource](#read-a-resource)
     * [Update a partial resource](#update-a-partial-resource)
     * [Avoiding concurrent updates](#avoiding-concurrent-updates)

      
 Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc.go)


## Conventions 

Keywords have been adopted from [RFC conventions](https://tools.ietf.org/html/rfc2119) to signify requirements. 

##Acknowledgments
Current Maintainer is <joyce.stack@mendeley.com>

Original documented drafted and attributed to Matt Thomson in 2014. 

This document borrowed heavily from:

* [JSON API](http://jsonapi.org/format/) 
* [Pay Pal](https://github.com/paypal/api-standards)
* [Mark Nottingham Blog](https://www.mnot.net)



## Resources

### Applies to all resources
####Dates

ISO 8601 format **MUST** be used for all dates and times e.g. 2015-02-18T04:57:56Z

**!** Unfortunately, some standard HTTP headers use their own format, defined in [RFC 2822](http://www.rfc-base.org/txt/rfc-2822.txt): Thu, 01 May 2014 10:07:28 GMT

Server-generated dates **MUST** be used everywhere as the server is the only reliable clock.


### Collection Resources

Collections of resources can be operated on using HTTP verbs. 


`GET` - Fetch a representation of the resource at the URI.

`POST` - Store the enclosed entity as a subresource of the resource at the URI.

`PUT` - Store the enclosed entity under the URI.

`DELETE` - Delete the resource at the URI.

`PATCH` - Apply partial modifications to the resource at the URI.

**Naming**

Resources **MUST** be identified using plural nouns. A single resources **MUST** be identified using a single noun. 


	* /things - identifies a collection of resources.
    * /things/(resource_identifer) - identifies a single resource within a collection


**Namespaces**

Each collection of URI templates **MUST** include a namespace at the start of the URI. This namespace identifies a collection of related resources. This avoids collisions when there is multiple resources with similar names but different functionality.  

**URI Templates**

URI templates **MUST** follow either the URI template

<code>/{namespace}/{resource}/{resource_identifier}/{sub_resource}/{sub_resource_identifer}/</code>



**HTTP Cheat Sheet**

|  | GET | POST | PUT | DELETE | PATCH | 
| ---| --- | --- | --- | --- | --- | 
| How does the HTTP spec define it? | The GET method requests transfer of a current selected representation for the target resource. | The POST method requests that the target resource process the representation enclosed in the request according to the resource’s own specific semantics. | The PUT method requests that the state of the target resource be created or replaced with the state defined by the representation enclosed in the request message payload. | The DELETE method requests that the origin server remove the association between the target resource and its current functionality. | The PATCH method requests that a set of changes described in the request entity be applied to the resource identified by the Request-URI. (RFC 5789) |
Is it safe? (no side effects) | Yes | No | No | No | No | Yes | Yes | No | Yes |
Is it idempotent? (repeating the request may give a different response, but makes no material difference to the resource) | Yes | No | Yes | Yes | No | Yes | YEs | No | Yes |
Are responses cacheable? | Yes | Only when explicitly marked as such | No | No | Only when explicitly marked as such | No | Yes | No | No |
Can requests include a body? | No | Yes | Yes | No | Yes | Yes (but no defined uses for this) | No | No | No |
What could it be used for in a RESTful API? | Fetching a representation of the resource | Creating a new resource from a representation | Replacing a resource with a representation | Deleting a resource | Applying partial updates to a resource | CORS | Fetching only the headers (e.g. pagination counts) | Nothing - breaks layered system constraint | Nothing - breaks layered system constraint       


             
             
#####Filtering 

All resources **SHOULD** provide a filtering mechanism where it is expected to return a large collections. 

Query parameters **MUST** be used to filter on resources. 

	* e.g. /dogs?type=poodle  

             
#####Sorting

A resource that is required to sort its collection based on some criteria **MUST** use the keyword <code>sort</code>

	* e.g. /dogs?sort=type
	
	
Sort order **MUST** use the <code>order</code> query parameter to indicate the sort order. The values **MUST** be <code>asc</code> or <code>desc</code>
 
             
#####Pagination 

Collection resources **MUST** have an upper bound on the number of items in the response. 

Clients **MUST** use **cursor-based** pagination. [Link headers](https://tools.ietf.org/html/rfc5988) are used to link to other pages. Using Link headers keeps resource representations clean. 

	GET /documents
    200 OK
    Link: </documents?marker=291d3064-4f74-4932-bfc8-4277d441705b>; rel="next";
    [
    ]
    // do

<code>limit</code> **MUST** be used to indicate the upper bounded value. 

#####Time selection queries
modified_since or deleted_since or {property_name}_since **SHOULD** be provided if time selction is needed. 
            
#####Bulk requests
Developers **SHOULD NOT** write bulk APIs. 

e.g. <code>GET /dogs/id1,id2,id3,</code>         

*Why?*

* different response formats from the same URI <code>GET /dog/id1</code> should return a single resource and not a collection of resources. 
* breaks cacheability
* URL doesn’t represent a resource
* URLs have a maximum length    


------
             
###Create resource    
Creates a new resource using the POST verb. The response to a POST **MUST** be `201 Created`, with a `Location header` containing the URL where the resource can be found. The body **MUST** contain a representation of the resources (including any server-generated fields). 

*URI template*

<code>POST /{namespace}/{resource}/</code>

*Example request and response*
 	POST /datasets/drafts
    {"field": "value"}

	    201 Created
	    Location: https://api.mendeley.com/datasets/drafts/291d3064-4f74-4932-bfc8-4277d441705b
	    {"field": "value", "created": "2015-02-18T04:57:56Z"}    
    
    
Clients **MAY** use the [HTTP Prefer](http://tools.ietf.org/html/rfc7240) header to give a client a mechanism to return a full representation or not:

        Prefer: return=minimal
        Prefer: return=representation

The body of a `POST` request, and the body returned on a GET request to the URL from the `Location` header, should have the similar structures.

       POST /things {"field": "value"} 
       
       201 Created
       Location: https://api.mendeley.com/things/291d3064-4f74-4932-bfc8-4277d441705b

       GET /things/291d3064-4f74-4932-bfc8-4277d441705b 
       
       200 OK
       {"field": "value", "created": "2015-02-18T04:57:56Z"}

GET response will usually have more fields, but that’s OK. There are some some cases where this doesn’t work e.g. 

-   POST /files: body is the file bytes
-   GET /files/(id): body is the file bytes
-   extra metadata (e.g. the document that the file is attached to) goes in the headers as it’s the only place left

          
#####Preventing duplicate creation of resources

Clients **MAY** using the [*post once exactly*](https://tools.ietf.org/html/draft-nottingham-http-poe-00) pattern to avoid duplicates being created. 

	POST /poe/documents

    200 OK
    Location: /poe/documents/291d3064-4f74-4932-bfc8-4277d441705b

    ￼￼POST /poe/documents/291d3064-4f74-4932-bfc8-4277d441705b
    {"title": "Underwater basket weaving"}

    200 OK
    Location: /documents/7ab2c167-8e48-4fb8-85b0-73cdb8662a64

    ￼POST /poe/documents/291d3064-4f74-4932-bfc8-4277d441705b
    {"title": "Underwater basket weaving"} 

    405 Method Not Allowed
    Allow: GET
          
         
This is just a draft and has not been updated in 10+ years.       
         
         
#####Linking/Unlinking resources together 
You **MAY** Link headers to indicate when one resource is linked to another. It is used in our current pagination mechanism. 
  
*Example request*   
If you had to create a 'Dog' resource and link it to an 'Owner' then you could do:           
             
	￼￼￼POST /dogs
	Link: </owners/291d3064-4f74-4932-bfc8-4277d441705b>; rel="owner";             
             
             
*Example in pagination*
This shows the link to the 'next' item in the list. 

	GET /documents
    200 OK
    Link: </documents?marker=291d3064-4f74-4932-bfc8-4277d441705b>; rel="next";
    [
    ]
    // do             

You **MAY** use the UNLINK header to unlink one resource from another. 
             
Specified by [*this internet draft*](http://tools.ietf.org/html/draft-snell-link-method), but it didn’t make it into the HTTP spec.              
             
             
###Read a resource       
Reads a single resource using the GET verb from a collection of resources. 

GET calls can be called multiple times without any side effects i.e. idempotent.

The response to a GET **MUST** be `200 OK`. The body **MUST** contain a representation of the resources including any server-generated fields. In the event the resource is not located then the HTTP status returned **MUST** be `404 Not Found`. 


*URI template*

<code>GET /{namespace}/{resource}/{resource_identifier}</code>

*Example request and response*

 	GET /datasets/articles/{id}
    
	{
		"id": "53c9523b-7535-4501-9969-93706f14672a",
		"title": "Distinct loci of lexical and semantic access deficits in aphasia: Evidence from voxel-based lesion-symptom mapping and diffusion tensor imaging",
		"doi": "10.1016/j.cortex.2015.03.004",
		"journal_id": "7be5c683-a7c2-4fe3-95c8-947b58706f2b"
	}


###Update a partial resource 
Updates a single resource using the PATCH verb.  

The response to oa PATCH **MUST** be `200 OK`. The body **MUST** contain a representation of the resources including any updated server-generated fields.


*URI template*

<code>PATCH /{namespace}/{resource}/{resource_identifier}</code>

*Example request and response*

 	PATCH /datasets/drafts/{id}
    
	{
     "name": "Testing One Two"
    }




###Avoiding concurrent updates
Clients **MAY** use a precondition check of `If-Unmodified-Since` header on update requests. If specified, the resource in question will not be updated if there have been any other changes since the timestamp provided. Should be specified in [RFC 2822](http://www.rfc-base.org/txt/rfc-2822.txt) format e.g. Thu, 01 May 2014 10:07:28 GMT


It **MAY** be a requirement and the server can send back `428 Precondition Required` if there is no `If-Unmodified-Since` header.

This **MAY** be used for GET requests e.g don’t download data that hasn’t changed since the client last requested it




