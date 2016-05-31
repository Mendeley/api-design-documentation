Deprecating an API and Blackout Testing Template
------------------------

Introduction
---------------------
Twitter carried out their first blackout test in March 2013 to help application developers understand the impact the retirement would have on their applications a their users. We adopted this approach to gracefully transition multiple clients off of old APIs.This  is also can be seen as a dry run for all teams before the big day. 
Here is a template of the activities that you need to schedule in. You can also read a blog post here that I wrote.  


##Picking the Date of API breakages and the Blackout day
Typically, we have given our clients 3 months notice of any significant breaking changes. This is what clients have come to expect so I would encourage that we stick to this. We can't expect clients to move at the same pace as us and it should allow the product team to schedule in the work each quarter.  The  blackout tests are run about 2-3 weeks before the API breaking change is expected to occur.  

###Prerequisites 
[ ] Check with internal teams that they don't have any massive releases going out 

[ ] Check that QA are happy with the suggested date and it doesn't impact any testing releases they may have. You will require their support. 

[ ] Prepare any config changes you need to make to be able to control the blackout testing - e.g:  you will need to, at a minimum, turn it off, back on, and then off again, all without a redeploy, turn off any Jenkins auto-deploys etc 
			
[ ] Announce the breaking change and blackout day to internal and external clients 
Joyce Stack can help with the external clients. 

- This should include the **exact dates and time of each activity** and the **exact HTTP response codes** that the clients will expect to see during the 1 hour testing and when the change happens for real. 
- Once announced we should endeavour to commit to the dates - the only reason we might not go ahead as scheduled is due to security or legal reasons. 
- Prepare any Kibana dashboards so you can monitor activity 
- Disable API proxy deployments (to make sure our config change doesn't get overwritten)
 

###Running the test
- Deploy the config change
- Check out a "config-change-..." branch 
- Check the Kibana dashboards to make sure that the expected HTTP response codes are occurring 
- QA to run the required tests through the gnome test tool - co-ordinate with QA to define the correct test subset to run.
- Sanity check all systems
- Check API support page
- Check other Kibana dashboards such as API Errors, Access Logs and Service Logs 

###Ending the test
- Back out the config change and push


###After the test
- Get a list of clients who were affected by the test and get in touch with them - Joyce Stack can help with external clients 
- In the event of any significant problems then a retrospective should be run.  We may have learned that we can't apply the breaking change OR we may have to reschedule it. 



