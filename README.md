# **USSD Flow**:
[PHP code sample for reference](https://build.at-labs.io/docs/ussd%2Foverview)

```
		-— ussd —> 					-—POST req—> 
Phone 				 AT API Cloud	 			    Our API
		<-— menu -—				    <-— text -—
````

## Our API:
* This should be developed by us to handle the AT POST request and send response as plain text.
* Service code and our API URL should be registered in AT.
* **API Params**:
    - `sessionId`		- a unique value generated when session starts and user responds to the given menu
    - `phoneNumber`	- mobile number of subscriber interacting with USSD app
    - `networkCode`	- telco of phoneNumber
    - `serviceCode`	- USSD code assigned to our api
    - `text`			- This shows the user input. It is an empty string in the first notification of a session. After that, it concatenates all the user input within the session with a * until the session ends.

## USSD Notes:
- USSD is session driven. Authenticate using sessionId from post request.
- To continue the session, CON should be starting text in the response. Else, END should be the starting text.
- USSD session will be terminated if our API is not working or the response malformed (does not begin with CON or END)
 
-----------------------
>USSD Flow ends here
-----------------------

 # **SMS Flow**:

 ```
		    --POST req—> 					-Send SMS—> 
Our API 				  AT API Cloud   	 			   Phone
		    <-—JSON res-—				    <-—Status update-—
        status update
````
### **Description:**
- Can send SMSs from our application by making a HTTP POST request to the SMS API.
- For each request sent from our application, service will respond with a `notification` back indicating whether the sms transaction was `successful` or `failed`.

### **Send Bulk SMS:** 
[Reference for code/request](https://build.at-labs.io/docs/sms%2Fsending%2Fbulk)

**Endpoints for Testing:**
- [Live](https://api.africastalking.com/version1/messaging)
- [Sandbox](https://api.sandbox.africastalking.com/version1/messaging)

- **Request Params:**
    - `username`: AT app username
    - `to`: Recipients' phone numbers
    - `message`: message text
    - `from` *(optional)*: default=AFRICASTKNG
    - `bulkSMSCode`: value must be set to 1 for bulk messages (account will be charged based on this value)
    - `keyword`: used for premium service
    - `linkId`: used for premium services to send OnDemand messages. SMS Service will forward the linkId to our application when the user sends a message to our service
    - `etc` - includes few other optional inputs

- **Response:**
````
{
    "SMSMessageData": {
        "Message": "Sent to 1/1 Total Cost: KES 0.8000",
        "Recipients": [{
            "statusCode": 101,
            "number": "+254711XXXYYY",
            "status": "Success",
            "cost": "KES 0.8000",
            "messageId": "ATPid_SampleTxnId123"
        }]
    }
}
````
