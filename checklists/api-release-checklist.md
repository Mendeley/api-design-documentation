Releasing version 1 of an API endpoint 
------------------------------------

All endpoints should have completed the [BETA checklist](https://github.com/Mendeley/api-design-documentation/blob/master/checklists/beta_checklist.md) before moving onto releasing the API to version 1.  

Not all items will be relevant in all cases but most should be.  

Borrowed items from this [API checklist](https://mathieu.fenniak.net/the-api-checklist/)


## API checklist 

### Why? 

[ ] Does it enable features across multiple platforms? Is their one there already doing a similar thing? Can you extend an existing one? Can you throw away one?  

### What?  

[ ]  Are all use cases of this feature addressed? Does it enable a complete feature? Have product signed off on it? 

[ ] Is the API stable? Have you appropiate monitoring in place? 

[ ] Does the API play a key role in your overall business strategy? (e.g. more users)


### General
[ ] Does it conform to the API standards document? 

[ ] Have you considered other clients use cases? Can they use these APIs? e.g. mobile. APIs service more than one client.  

[ ] Have you tested your versioning stratgegy before releasing? How will you communicate changes?  

[ ] Is the API authenticated? 

[ ] Is it documented for internal and external audiences? 

[ ] JIRA gardening exercise to ensure that we have no lingering tickets for this feature that needs consideration e.g.removing deprecated fields etc? 

[ ] Is it internal only? If yes, then how will other internal clients find out it exists? Have you documented it somewhere else? It should be more than Swagger as this is **not** a documentation tool.   

[ ] Ongoing support and maintenance budgets? Who will support 3rd party clients? What level of staffing and engagement are you willing to fund to support the public developer community? 

[ ] How will it get prioritised against the other products roadmaps? 

### Legal 
[ ] Have you considered Terms of Service, Privacy Policy, Copyright, Data License Cookies?    

### Design 

[ ] Have you read the URLs section of the [API design document](https://github.com/Mendeley/api-design-documentation#urls) and applied them? If you find an undocumented use case please raise an issue in GitHub  



### Other 

[ ] Have you got your API reviewed by the API Steering Group? 

[ ] Is their a feedback channel that consumers of the API can report to?  

[ ] A blog post on the API blog to introduce the new endpoint and why it will help clients is a nice to have.  





 