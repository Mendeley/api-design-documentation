- [Collection Resources](#collection-resources)
  - [Collection Resource](#collection-resource)
    - [Filtering](#filtering)
      - [Paging](#paging)
        - [Hypermedia links](#hypermedia-links)
      - [Time selection](#time-selection)
      - [Sorting](#sorting)
  - [Read Single Resource](#read-single-resource)
  - [Update Single Resource](#update-single-resource)
  - [Update Partial Single Resoure](#update-partial-single-resoure)
  - [Delete Single Resource](#delete-single-resource)
  - [Create New Resource](#create-new-resource)
  - [Create New Resource - Consumer Supplied Identifier](#create-new-resource---consumer-supplied-identifier)
  

## Conventions 
[RFC conventions](https://tools.ietf.org/html/rfc2119)



## Collection Resources

Collections of resources can be operated on using HTTP verbs. 


`GET` - Fetch a representation of the resource at the URI.

`POST` - Store the enclosed entity as a subresource of the resource at the URI.

`PUT` - Store the enclosed entity under the URI.

`DELETE` - Delete the resource at the URI.

`PATCH` - Apply partial modifications to the resource at the URI.

**Naming**

Resources **MUST** follow the naming stanard of plural nouns. Single resources **MUST** follow the naming standard of a single noun. 


	* /things - identifies a collection of resources.
    * /things/(uuid) - identifies a single resource within a collection.


**Namespaces**
(should we map it to capabilites like they do in the architecture team - whats the difference? )

Each collection of URI templates **MUST** include a namespace at the start of the URI. This namespace identifies a collection of related resources. This avoids collisions when there is multiple resources with similar names but different functionality.  

**URI Templates**

URI templates 

/namespace/resource/


**HTTP Cheat Sheet**

|  | GET | POST | PUT | DELETE | PATCH | 
| ---| --- | --- | --- | --- | --- | 
| How does the HTTP spec define it? | The GET method requests transfer of a current selected representation for the target resource. | The POST method requests that the target resource process the representation enclosed in the request according to the resource’s own specific semantics. | The PUT method requests that the state of the target resource be created or replaced with the state defined by the representation enclosed in the request message payload. | The DELETE method requests that the origin server remove the association between the target resource and its current functionality. | The PATCH method requests that a set of changes described in the request entity be applied to the resource identified by the Request-URI. (RFC 5789) |
Is it safe? (no side effects) | Yes | No | No | No | No | Yes | Yes | No | Yes |
Is it idempotent? (repeating the request may give a different response, but makes no material difference to the resource) | Yes | No | Yes | Yes | No | Yes | YEs | No | Yes |
Are responses cacheable? | Yes | Only when explicitly marked as such | No | No | Only when explicitly marked as such | No | Yes | No | No |
Can requests include a body? | No | Yes | Yes | No | Yes | Yes (but no defined uses for this) | No | No | No |
What could it be used for in a RESTful API? | Fetching a representation of the resource | Creating a new resource from a representation | Replacing a resource with a representation | Deleting a resource | Applying partial updates to a resource | CORS | Fetching only the headers (e.g. pagination counts) | Nothing - breaks layered system constraint | Nothing - breaks layered system constraint       


             