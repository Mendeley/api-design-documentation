# API Design & Best Practices 

This is a document providing suggestions, best practices and examples for writing APIs. 

This is to encourage consistency, maintainability, and best practices. 

Why? 

To avoid [bikeshedding](https://en.wikipedia.org/wiki/Parkinson%27s_law_of_triviality) arguments and to aid a positive developer experience (DX). 
 
======== 
## Current Maintainer
Joyce Stack

Original documented drafted and attributed to Matt Thomson. 

========

## Who should read this document? 
This document is intended for any developer who is writing an API.  

========

## Conventions used

**✔** We do this, we like it. 

**X** We don't do this, but that's fine because I don't like it anyway

**!** We do this, but I wish we didn't 

**?** This is something that we haven't done that's worth at least
 thinking about. It may still be a terrible idea.


========

## General Advice  

* Read [this book](http://www.amazon.com/RESTful-Web-Services-Leonard-Richardson/dp/0596529260)
	* We might not follow all advice in this book, but it's still worth a read.
* Use other APIs for inspiration (but don't copy blindly)
	* Most of the problems we try to solve are not unique to us We like Twitter, Stripe, Twilio, ...
	* Just because someone else is doing it doesn't mean it's right, or even that the people who put it there are happy with it
	* Right now someone in Twitter is giving a talk on things they wish they'd done differently in their API
* A tidy API is more important than tidy code
	* Design the API that your clients want - there are more of them than us. 
	* Might make implementation more complicated but usually worth it
	* Exception: database constraints (hard to change schemas on our big tables, had to make compromises)
* Look before you leap
	* APIs can be hard to change after they're released 
	* We can move very quickly, but clients can't always 
	* Some decisions are hard to change


========

## Guiding principles

### Support a wide range of clients

* Network
	* Same Amazon region as the API 
	* Data centre with a fibre link 
	* Home internet connection 
	* Mobile network
	
* Device - multiple devices: Differ vastly in power, battery.
	* Servers 
	* PCs
	* Mobile phones / tablets
	
* Programming languages - languages: not all languages have the same support for concurrency/HTTP features

* Syncing vs non-syncing - Getting lots of data vs on-demand

* Release cycles -  can vary widely between webapps (days) and installed apps (months when you consider how long it takes people to upgrade)


### Be consistent 
* Should look like one API, not many.
* Similar things should look similar, regardless of which service they're in, or who worked on them.
*  Examples:
	* prefer snake_case to camelCase or kebab-case
	* a person has a first_name and a last_name **(&#33; - should think about this more - not all names follow this standard e.g. Chinese)**
	* when to use "link", "website" and "url"
	
	
### Include all non-trivial business logic
* Having logic spread amongst all of our clients makes it much harder to:
	* achieve cross-device consistency
	* write new apps	
* Always prepare for a second client even if you're building for one
* Clients should be little more than a view on the data. That data, and the bulk of the logic, should be in the API

### Follow standards pragmatically
* Use standard HTTP, and industry best practices, where appropriate:
	* avoids surprising client developers
	* tried and tested
	* better support in client-side libraries 
	
* However, there may be times where:
	* following the standard exactly would be excessively complicated
	* there is no standard
	* there are multiple competing standards, and vigorous debate about which one is right

**More important to have a working API with actual clients, than achieve perfect RESTful purity**


========	

## REST 

### What is REST? 
* Based on a research paper of [Roy Fielding](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm) 
* Read [this book](http://www.amazon.com/RESTful-Web-Services-Leonard-Richardson/dp/0596529260)
* See [Wikipedia](https://en.wikipedia.org/wiki/Representational_state_transfer)

### REST constraints 
* Client-server
	* a uniform interface separates clients from servers.
		* clients don't have to worry about data storage (improves portability), well-defined interface means that servers and clients can evolve independently
* Stateless
	* no client context is stored on the server between requests.
		* improves scalability, clients can have local state
* Cacheable
	* clients can cache responses.
		* responses can define themselves as cacheable or not, improves performance
* Layered system
	* a client cannot ordinarily tell whether it is connected directly to the end server, or to an intermediary along the way.
		* load balancing, caching, securiy
* Code On Demand (Optional)
	*  	servers changing client behaviour by sending code to clients (e.g. Java applets). 
		* We don't do this

* Uniform interface
	* This has 4 constraints which are fundamental to building REST services so including a new section. 
	
	
	
### Uniform Interface	

* Identification of resources
	* each resource is identified by a URI, each request relates to a resource separation between resource and representation (JSON/XML)
* Manipulation of resources through representations
	* a client that holds a representation has enough information to modify/delete the resource
* Self-descriptive messages
	* message holds enough information about how to process it
* [Hypermedia as the engine of application state (HATEOAS)](#Hypermedia)
	* clients make state transition using links in the messages warning - here be dragons

### Addiontal notes on Uniform Interface

* Uniform interface decouples the architecture, allows client and server to evolve independently
* Resources are the entities in the system, representations are the message contents
* Messages describe the state of the resources, and how they should change 
* REST usually runs over HTTP, but it doesn't have to. I'll assume you know about HTTP, and talk about how we use it to design APIs.

### What does this mean in practice?
* Interfaces need to be clearly defined, to allow clients and servers to evolve independently

* URLs need to be chosen carefully

* Use appropriate HTTP methods to manipulate state
 
* Use standard HTTP headers to indicate how messages should be processed
	* e.g. Content-Type header indicates which parser to use
	* e.g. Cache-Control header indicates how long to cache

* Think in terms of maniuplating resources, rather than making remote procedure calls
	* e.g. change the "is read" flag on a document, rather than marking a document as read

* Think about making resources cacheable

### Hypermedia
API design strategy for connecting resources within APIs.

* each response includes links to related/nested resources, e.g.
	* group has link to list of group members document has link to attach a file
* clients don't hard-code URLs, they start at the root and navigate to the thing that they want
	* navigating hypermedia is like browsing a website
	* if you want to build a useful client, instead of hard-coding URLs, you end up hard-coding which links to follow
	* it is becoming more popular with more success stories in the wild such as the [Guardian](http://www.programmableweb.com/news/how-guardian-approaching-hypermedia-based-api-infrastructure/2015/04/27) 
	
	
========	
	
## HTTP 

### Use the right spec 
 
**Don't use RFC2616.** Delete it from your hard drives, bookmarks, and burn (or responsibly recycle) any copies that are printed out. 

RFC2616 was the reference for HTTP, but now it's deprecated. You'll still see it referred to in lots of places.

Source: [mnot's blog](https://www.mnot.net/blog/2014/06/07/rfc2616_is_dead)

Reworked into these six RFCs:

* [RFC7230](https://tools.ietf.org/html/rfc7230) - HTTP/1.1: Message Syntax and Routing
	* low-level message parsing and connection management
* [RFC7231](https://tools.ietf.org/html/rfc7231) - HTTP/1.1: Semantics and Content 
	* methods, status codes and headers
* [RFC7232](https://tools.ietf.org/html/rfc7232) - HTTP/1.1: Conditional Requests 
	* e.g., If-Modified-Since RFC7233 - HTTP/1.1: Range Requests - getting partial content
* [RFC7234](https://tools.ietf.org/html/rfc7234) - HTTP/1.1: Caching 
 	* browser and intermediary caches 
* [RFC7235](https://tools.ietf.org/html/rfc7235) - HTTP/1.1: Authentication 
 	* a framework for HTTP authentication

========

 
## URLs 

### Basics
URLs are based around plural nouns.

	* /things - identifies a collection of resources.
	* /things/(uuid) - identifies a single resource within a collection.

Should only need one UUID per URL - "universally unique"

UUIDs rather than MySQL keys - don't couple the API to our database


### Filter with query parameters
**✔** We do this, we like it.

	 /documents?starred=true

	 /files?document_id=b91d9530-9017-4b96-b000-cdc245920355

We **don't** like 
 
 	/documents/b91d9530-9017-4b96-b000-cdc245920355/files/

* reduces surface of the API
* makes it possible to get all files without having to traverse documents (better for syncing clients)

**?** We are doing this, it maybe a terrible idea

	/locations?prefix=XXX
	
	/institutions?REQUIRED_FILTER
	
	/catalog?REQUIRED_FILTER
* in some cases we have required query parameters e.g. prefix in locations and doi, pmid etc in catalog
* we do this because returning the collection of resources would be too big for most clients e.g. the catalog

### Prefer flat to nested
**✔** We do this, we like it.

	 /files/0c98cfaa-7d58-4720-abd1-3b09fe008c48
	 
We **don't** like 

	/documents/b91d9530-9017-4b96-b000-cdc245920355/files/0c98cfaa-7d58-4720-abd1-3b09fe008c48	 
* reduces coupling between resources


### If you have to nest

**!** We do this, but we wish we didn't 

	/folders/1016198b-f97a-4123-82af-be7239ff352b/documents/8d7a4bd2-918a- 4a8c-8529-4ca437b91313

**X** We don't do this, but that's fine because we don't like it anyway

	/oapi/library/documents/7135271181/file/3221525ea6f746b577b6a8ad40d89df3f41f776a/2589311

* provide context by putting something between the IDs

* remove as many extraneous parts of the URL as you can

* think again about whether you really have to nest - can you identify the
resource by one UUID?

	* Second URL - IDs are document, filehash, group 
	* Not clear what that last ID is
	* The resource in question is a file - why can't it have an ID and be
 identified by just that?

### Actions
Some actions don't naturally fit a RESTful model.

As an emergency manoeuvre, use a POST with a verb in the URL.

 **!** We do this, but I wish we didn't  
 
 	POST /trash/8d7a4bd2-918a-4a8c-8529-4ca437b91313/restore
 
* this is the only time you're allowed to put a verb in the URL
* prefer "/actions" - if you have multiple then "restore" is like an ID, so should have something in the middle

 **?** This is something that we haven't done that's worth at least
 thinking about. It may still be a terrible idea.
 
 	POST /trash/8d7a4bd2-918a-4a8c-8529-4ca437b91313/actions/restore

 **X** We don't do this, but that's fine because we don't like it anyway 
 
 	RESTORE /trash/8d7a4bd2-918a-4a8c-8529-4ca437b91313
 	
 * this should be a last resort - prefer to PATCH the resource if you can.
 * however tempting it may be, don't invent your own HTTP method - some clients won't be able to handle this	
 
 
### Looking up by a secondary ID
**!** We do this, but I wish we didn't  

	/catalog?doi=10.1016%2Fj.molcel.2009.09.013

* /catalog is a collection resource, and needs to return a list (e.g./catalog?author=John+Smith).

* But, with a DOI, this can only a list of 0 or 1 documents.

* Would be more natural to have something that could return a 200/404
status.

**?** This is something that we haven't done that's worth at least
 thinking about. It may still be a terrible idea.

	/catalog;doi=10.1016%2Fj.molcel.2009.09.013

[*Matrix URIs*](http://www.w3.org/DesignIssues/MatrixURIs.html)

* Does exactly the thing we want

* Based on a 1996 blog post by Tim Berners-Lee - not in any RFC

* Not widely implemented

	* Jersey does support it (@MatrixParam), but most other frameworks don't
	
 
======== 
 
## HTTP Methods 

### GET

* Fetch a representation of the resource at the URI.

### POST

* Store the enclosed entity as a subresource of the resource at the URI.

### PUT

* Store the enclosed entity under the URI.

### DELETE

* Delete the resource at the URI.

### PATCH

* Apply partial modifications to the resource at the URI.
	* PATCH wasn't in the original specification, but we use it.



### Safety and idempotency

A method is **safe** if it has no side effects.

* GET
	* retrying has same response 	

A method is **idempotent** if the side effects of repeating the
request are the same as for a single request.

* GET

* PUT

* DELETE

	* retrying may have different response (e.g. already deleted), but doesn't make any material difference

Otherwise, no guarantees about what repeating a request will do.

* POST

* PATCH (but often idempotent in practice)

	* won't be retried


Clients can use these facts to decide whether to retry a request.

### Which one to use?

* Use GET for retrieving data.

* Use POST for creating a new resource (not idempotent - multiple POSTs
create multiple resources).

* For updates:

 	* PUT - replace the whole resource with this representation. 
 	* PATCH - replace only the fields that are in the body.

We prefer PATCH to PUT for updates. **✔** We do this, we like it.
 
* Reduce window conditions caused by concurrent updates (more on this
later).

* Don't need to have the whole resource in hand to do an update.

* Can still get PUT-like semantics by sending the whole object.

* In reality, the client doesn't replace the whole resource anyway (e.g.
last modified time).


========

## Specials


### OPTIONS

Returns details about what requests will be accepted for a URL.

**✔** Used for CORS (more later).

### HEAD

Identical to GET, but returns only the headers, not the body.

**?** Clients could use this to get pagination counts, but we haven't
implemented that.

========

## Oddities

### CONNECT

**X** Converts the connection to a transparent TCP/IP tunnel.

### TRACE

Echoes the request back, so that the client can see what intermediate
servers have done.

**X** Breaks layered system constraint.

### LINK / UNLINK

Used to create/delete a link between two resources.

e.g. adding/removing documents from folders.

**?** Specified by [*this internet
draft*](http://tools.ietf.org/html/draft-snell-link-method), but didn't
make it into the HTTP spec.

### MISCELLANEOUSOTHERTHING

**X** You can make up your own methods, but please don't.

