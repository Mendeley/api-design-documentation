Releasing version 1 of an API endpoint 
------------------------------------

All endpoints should have completed the [BETA checklist](https://github.com/Mendeley/api-design-documentation/blob/master/checklists/beta_checklist.md) before moving onto releasing the API to version 1.  

Not all items will be relevant in all cases but most should be.  

Borrowed items from this [API checklist](https://mathieu.fenniak.net/the-api-checklist/)


## API checklist 

### Why? 

[ ] Is it internal only? If yes, then how will other employees find out it exists? Have you documented it somewhere else? 

[ ] Does it enable features across multiple platforms? Is their one there already doing a similar thing? Can you extend an existing one? Can you throw away one?  

### What?  

[ ]  Are all use cases of this feature addressed? Does it enable a complete feature? Have product signed off on it? 

[ ] Is the API stable? Have you appropiate monitoring in place? 


### General
[ ] Does it conform to the API standards document? 

[ ] Have you considered other clients use cases? Can they use these APIs? e.g. mobile

[ ] Is the API authenticated?

[ ] Is it documented on Swagger? 

[ ] Is it documented for internal and external audiences? 

[ ] Ongoing support and maintenance budgets? Who will support 3rd party clients?

### Legal 
[ ] Have you considered Terms of Service, Privacy Policy, Copyright, Data License Cookies?    

### Design 

[ ] Have you read the URLs section of the [API design document](https://github.com/Mendeley/api-design-documentation#urls) and applied them? If you find an undocumented use case please raise an issue in GitHub  

[ ] Have you provided a vendor-specific media type like below? Is the resource type used singular? 
		
		e.g. application/vnd.mendeley-<RESOURCE_TYPE>+json

[ ] For all list oriented or search endpoints then are they returning paginated results? 

[ ] Are you logging errors for your API? 

[ ] Are you using ISO 8601 date/time formats? 

[ ] Have you considered rate limiting for this API? 

[ ] Are your attributes adhering to the standards of JSON case? e.g. group_id and not groupId 

[ ] Are we leaking out our internal data models in our API? Expose resources and not database tables e.g. resource called OAuthClient 


### Other 

[ ] Have you got your API reviewed by the API Steering Group? 

[ ] Is their a feedback channel that consumers of the API can report to?  




 