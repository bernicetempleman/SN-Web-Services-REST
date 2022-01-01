# SNOW-Web-Services-REST

##Practical REST

### On source: Outbound->rest message
- Need from target:
- Needed to send data to target instance: 
1.	Endpoints Methods (Get, Put, POST, Patch, Delete) 
2.	Header 
3.	Content 
4.	Body 
5.	Authorizaton Details

###On Target: go to Rest API explorer
- POST
- https://devxxx.service-now.com/api/now/table/incident
- Select table name: Incident
- Scroll down: Request headers
- JSON header 

###Scroll/ Request Body 
- Add a field
- Caller id /(name)
- Short description/ test
- Stat/ in progress
- {"u_caller_id":"username","u_short_description":"this is a rest test","u_category":"ABC","sys_import_state":"In Progress"}
- Copy to notepad

### Authoirization
- Id and passwd

### From target
- // create user
- Demo.rest /pw 
- Add role rest_service
- Send
- Give details to source

### Create outbound rest message on source
- Outbound (under system webservices)-> outbound-> rest message

### Name: demo -inc creation
- Endpoint https://devxxxxx.service-now.com/api/now/table/incident
- Authentication: basic
- New/ User
- Save
- Default get /delete
- New method post
- Name: demo -incident creation
- HTTP POST
- Inherit from parent

### Http Method tab
- Content: from target note
- SAVE
- TEST
- Should create incident on target.

### auto trigger when incident created
- Replace  content in http method
-  {
- "caller_id":"${caller}",
- "short_description"${short_desc}:"",
- "category":"${category}",
- "state":"${state}"
- }

### Create Business rule after incident
- On target : go to rest api explorer
- Code sample at bottom
- Servicenow script or other
- var request = new sn_ws.RESTMessageV2();
-     business rule script:
- //copy name of rest message and http method of rest
-     var request = new sn_ws.RESTMessageV2('demo - inc creation', 'incident_creation');
-     request.setStringParameterNoEscape("caller", current.caller_id);
-     request.setStringParameterNoEscape('short_description', current.short_desc);
-     request.setStringParameterNoEscape('category', current.category);
-     request.setStringParameterNoEscape('state', current.state);
-     request.execute();
-     gs.addInfoMessage('testing inc rest');

### create incident on source and check target
