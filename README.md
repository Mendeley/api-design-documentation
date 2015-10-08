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

&#88; We don't do this, but that's fine because I don't like it anyway

&#33; We do this, but I wish we didn't 

&#63; This is something that we haven't done that's worth at least
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




