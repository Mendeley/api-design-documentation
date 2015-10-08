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

** More important to have a working API with actual clients, than achieve perfect RESTful purity**	
	

