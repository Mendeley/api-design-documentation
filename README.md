# API Design & Best Practices 

This is a document providing suggestions, best practices and examples for writing APIs. 

This is to encourage consistency, maintainability, and best practices. 

Why? 

To avoid [bikeshedding](https://en.wikipedia.org/wiki/Parkinson%27s_law_of_triviality) arguments and to aid a positive developer experience (DX). 
 
## Current Maintainer
Joyce Stack

Original documented drafted and attributed to Matt Thomson. 

## Who should read this document? 
This document is intended for any developer who is writing an API.  

## Conventions used

✔ We do this, we like it. 

**&#88;** We don't do this, but that's fine because I don't like it anyway

**&#33;** We do this, but I wish we didn't 

**&#63;** This is something that we haven't done that's worth at least
 thinking about. It may still be a terrible idea.


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
	
	
	
## Uniform Interface	

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

## Hypermedia
API design strategy for connecting resources within APIs.

* each response includes links to related/nested resources, e.g.
	* group has link to list of group members document has link to attach a file
* clients don't hard-code URLs, they start at the root and navigate to the thing that they want
	* navigating hypermedia is like browsing a website
	* if you want to build a useful client, instead of hard-coding URLs, you end up hard-coding which links to follow
	* it is becoming more popular with more success stories in the wild such as the [Guardian](http://www.programmableweb.com/news/how-guardian-approaching-hypermedia-based-api-infrastructure/2015/04/27) 
	



 
	

