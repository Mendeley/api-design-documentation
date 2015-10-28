
## Conventions 

Keywords have been adopted from [RFC conventions](https://tools.ietf.org/html/rfc2119) to signify requirements. 

##Acknowledgments
Current Maintainer is <joyce.stack@mendeley.com>

Original documented drafted and attributed to Matt Thomson in 2014. 

This document borrowed heavily from:

* [JSON API](http://jsonapi.org/format/) 
* [Pay Pal](https://github.com/paypal/api-standards)



## Resources

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
Developers **SHOULD NOT** write APIs. 

e.g. <code>GET /dogs/id1,id2,id3,</code>         

* different response formats from the same URI <code>GET /dog/id1</code> should return a single resource and not a collection of resources. 
* breaks cacheability
* URL doesn’t represent a resource
* URLs have a maximum length    
             
####Create a single resource    
         
             
             
             