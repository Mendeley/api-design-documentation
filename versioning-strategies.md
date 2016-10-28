## API Versioning Strategies 
These are my notes collected from various blogs and books. I have relied heavily on [Build APIs You Wont Hate](https://leanpub.com/build-apis-you-wont-hate) for outlining the pros and cons of each of the versioning strategies. 



[Roy T Fielding Quote on this issue](http://www.slideshare.net/evolve_conference/201308-fielding-evolve/31): 
> 
>     Roy Fielding states the best practice for versioning an API is: "Don't".



Some credits: 

* [HATEOAS](http://barelyenough.org/blog/2007/05/hypermedia-as-the-engine-of-application-state/)
* [Peter Williams - Versioning REST web services](http://barelyenough.org* /blog/2008/05/versioning-rest-web-services/)
* [Troy Hunt - Your API versioning is wrong](https://www.troyhunt.com/your-api-versioning-is-wrong-which-is/)
* [Ben Morris - REST APIs Dont Need a Versioning Strategy They Need a Change Strategy](http://www.ben-morris.com/rest-apis-dont-need-a-versioning-strategy-they-need-a-change-strategy/)
* [GitHub - API Strategy](https://github.com/restfulapi/api-strategy)
* [Roy T Fielding - Don't Do Versioning](http://www.slideshare.net/evolve_conference/201308-fielding-evolve/31)
* [Michael Pratt - Ways to Version Your API](http://urthen.github.io/2013/05/16/ways-to-version-your-api-part-2/)
* [Mark Nottingham, Bad HTTP API Smells: Version Headers"](https://www.mnot.net/blog/2012/07/11/header_versioning)
* Special mention for this book - [Build APIs You Wont Hate](https://leanpub.com/build-apis-you-wont-hate)


Criteria for picking a versioning strategy:
 
* evolvability 
* permalinking 


#Overview 

### URI 

Put a version in the URI e.g. ```api.example.com/v1/books/100```

* Pros
 	* easy to implement 
 	* easy to consume as a client developer 
 	* common practice across public APIs in the community 

* Cons
	* breaks evolvability principle as you can't evolve on the fly due to baked in versioning knowledge required at deployment time  	
	* does not represent the entity e.g. book with id 100 but a versioned representation of the entity. Implies that the v1 book is different to v2 book and not the same resource 
	* is not RESTful - see Roy's comment 
	* can lead to server side complexity issues single codebase v seperate codebases 
	* consumers have to do weird things to keep up to date with your API if they store any of the API call details at all
	
	
* Clients
	*  Etsy, Dropbox, Twitter, Bitly
	

Roy T Fielding Quote on this issue: 
> 
>     The reason to make a real REST API is to get evolvability...a "v1" is a middle finger to your API customers, indicating RPC/HTTP (not REST)


### Hostname 
Put a version in the hostname e.g. ```https://api-v1.example.org/books```

* Pros 
	* easy to implement 
	* easy to consume as a client developer 
	* can use DNS to split versions over multiple servers


* Cons
	* similar issues as with the URI 
	* seldom see this being done in the wild 
	* consumers have to change hostnames and possibly security settings to use new APIs


* Who does it? 
	* no idea  


### Body & Query Params  
Use it as an attribute in your JSON or use a query parameter e.g. ```GET /books { "version" : "1.0" }``` or ```GET /books?version=1.0```

* Pros 
	* the query parameter approach means you can make it optional for clients to call a new version or not - as long as you make it clear that 'latest' will be used when no version specific 
	* easy to implement 
	* easy to consume as a client developer 
	

* Cons
	* similar issues as with the URI and hostname approaches 
	* different content types require different params e.g. what if we have a Content Type of image/png then we would have to do this which is just not recommended  ```POST /images?version=1.0``` 
	* typically query parameters are used for filtering
	
	
* Who does it? 
	* Netflix, PayPal and Amazon SQS 	


### Custom Request Header  
Similar to the two former methods but you specify it in the request header e.g. ```curl -H "Accepts-version: 1.0" http://www.example.com/books```

* Pros 
	* maintains the URI space; semantically better because you are specifying <em>what</em> you want and not <em>how</em> you want it in the URI  

* Cons
	* [Interferes with Caches](https://www.mnot.net/blog/2012/07/11/header_versioning) 
	* more difficult to use due to the crafting of multiple HTTP headers
 
* Who does it? 


### Content Negotiation 

* What is it? 
* Pros 
* Cons
* Who does it? 


### Content Negotiation for Resources  

* What is it? 
* Pros 
* Cons
* Who does it? 


