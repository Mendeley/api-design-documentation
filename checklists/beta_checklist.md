Releasing a BETA API 
------------------------------------

All endpoints should start in Alpha while active development is being undertaken. Before applying the BETA tag on an API endpoint then here is a checklist to think about. Not all items will be relevant in all cases but most should be.  

Borrowed items from this [API checklist](https://mathieu.fenniak.net/the-api-checklist/)


## BETA checklist 

### Why? 

[ ] Have you considered why this API is valuable in the API program? Is it internal only? Does it enable features across multiple platforms? Is their one there already doing a similar thing? Can you extend an existing one? 

### What?  

[ ]  Are all use cases of this feature addressed ? Does it enable a complete feature? Have product signed off on it? 

[ ] Is the API somewhat stable? There maybe known bugs or weird corner cases. API changes are still possible if they are needed to address an issue. 


### General
[ ] Does it conform to the API standards document? 

[ ] Have you considered other clients use cases? Can they use these APIs? e.g. mobile

[ ] Is the API authenticated?

[ ] Is it documented on Swagger? 

### Design 

[ ] Have you read the URLs section of the [API design document](https://github.com/Mendeley/api-design-documentation#urls) and applied them? If you find an undocumented use case please raise an issue in GitHub  

[ ] Have you provided a vendor-specific media type like below? Is the resource type used singular? 
		
		e.g. application/vnd.mendeley-<RESOURCE_TYPE>.<VERSION>+json

[ ] For all list oriented or search endpoints then are they returning paginated results? 
[ ] Are you logging errors for your API? 

[ ] Are you using ISO 8601 date/time formats? 

[ ] Have you considered rate limiting for this API? 

[ ] Are your attributes JSON case? e.g. group_id and not groupId 

[ ] Are we leaking out our internal data models in our API? Expose resources and not database tables e.g. resource called OAuthClient 


### Other 

[ ] Have you got your API reviewed by the API Steering Group? 

[ ] Is their a feedback channel that consumers of the API can report to?  




 