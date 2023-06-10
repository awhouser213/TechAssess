Andrew Houser 										         6/10/2023
DocuSign Tech Assessment

The following repository contains the solutions for Andrew Houser’s technical assessment for DocuSign’s Technical Customer Success Manager (“TCSM”) position.

Original assessment is available here:
https://github.com/DS-CSE/TechAssess

Technical Assessment
This repository contains the materials for the DocuSign Global Customer Support candidate technical assessment.
Please utilize the contents of this repository to complete the following tasks:
1.	Modify the API call in API TEST 1 to send successfully.
2.	Attempt to make the API call in API TEST 2 successfully. Be prepared to explore this scenario in-depth during your technical interview.
3.	Create a successful API call to DocuSign that creates a single DocuSign envelope that combines the templates TestJSON1 and TestJSON2 and is directed to the following recipients: John Doe, johndoe@test.test and Jane Doe, janedoe@test.test.
When prompted, please return your completed API calls to your recruiting coordinator.
NOTICE
Please be prepared to discuss your progress during the technical portion of your interview, up to and including running the API calls live during the interview. If you were unsuccessful at any portion of this assessment, please be prepared to discuss the obstacles you encountered and the steps you attempted to move forward.

Prerequisite: 
•	Create a DocuSign Developer account
•	Install Postman
•	Configure DocuSign API Postman Collection 
See the Appendix Section for a step-by-step guide.
API TEST 1
1.	Modify the API call in API TEST 1 to send successfully.
 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/81a0f878-9ba7-4d0a-8ce8-123378773649)

Figure 1- Raw code for API TEST 1 before any edits
Initial Observations:
•	Header is a POST request to DocuSign envelope 
o	POST requests are adding data to the server 
o	Body contents contain metadata about recipients/signers
o	Assumption: some form of contract is sent to two signatories
•	Body contains a few possible errors:
o	First signer email address domain name appears invalid: ends with @jakedstest.xyz , not a .com, .edu etc.
o	Assumption: each signer is a different person
	recipientID for both signers are the same
o	Signer 2 email parameter is  blank
o	Only 1 document is sent, however each signer has received a unique documentID.
Steps Taken:
•	Converted documentBase64 to physical document via https://base64.guru/converter/decode/file
•	Note: file is a blank document.  If this were a real example this would be a call-out.
•	Paste code as-is to Postman:
 
Figure 2- Raw code for API TEST 1 before any edits, in Postman environment
Error 0: data not recognized as JSON
Step Taken: edit “header” field to contain Content-Type: application/json

Error 1:
 
Figure 3- First error message: invalid xPosition

Step Taken: removed the “-“ for recipient 2 xPosition
Note: may need to verify correct position with document designer

Error 2:
 
Figure 4- Second error message: duplicate recipients

Steps Taken: changed recipientID from “1” to “2” for second recipient

Error 3:
 
Figure 5- Third error message: Envelope is missing parameter

Step Taken: Added a new field below "emailSubject": "Simple Signing Example"    

Error 4:
 
Figure 6- Fourth error message: DocumentID is incorrect

Steps Taken: First recipient contains a value for documentID of “2”, while documentID specified in documents section is “1”.  Updated recipient line to documentID 1.



Error 5:
 
Figure 7- Fifth error message: Page number 2 does not exist

Steps Taken: The document itself is only 1 page (validated with base64 explorer site).  
Updated recipient 2-tab pagenumber from “2” to “1”

Error 6:
 
Figure 8- Sixth error message: invalid email address

Steps Taken: Added a valid email address to recipient 2











Updated Code:
 
Figure 9- Updated code
 
Figure 10- Success message received in Body
Validation:
•	Replaced sample email with personal email address to test
 
Figure 11 – Inbox containing DocuSign Envelope

 
Figure 12 – Document within DocuSign Envelope

Further Enhancements/ Notes: 
•	Saved documentBase64 as a global variable to improve readability


API TEST 2 
Attempt to make the API call in API TEST 2 successfully. Be prepared to explore this scenario in-depth during your technical interview.
 
Figure 13 – Raw code for API TEST 2 before any edits

Initial Observations:
•	Header is a POST request to DocuSign envelope 
o	POST requests are adding data to the server 
o	Body contents contain metadata about recipients/signers
o	Assumption: some form of contract is sent to two signatories


Steps Taken:
•	Created new Request in Postman, copy/pasted code as is and ran
•	Initial Run ran without an error message

 
Figure 14 – Raw code for API TEST 2 in Postman
 
Figure 15 – Success message received in body for as-is run

Based off the successful as-is run, the question now is: “what is the functional goal here?”.

Possibility #1: Hide a Document in Envelope from Recipient
•	enforceSignerVisibility set to true provides a hint.  
o	Parameter is typically set in the context of excluding recipients from viewing certain documents .  
•	As an envelope can contain many documents, excludedDocuments parameter should be in place to specify which document is hidden from which recipient.
Assumption:
•	For an example, let’s add a second document to the envelope and exclude Bruce Wayne from viewing.  
•	Barry Allen should see both files (test1.pdf and test2.pdf) while Bruce Wayne should only see test1.pdf
•	This requires adding an additional document to documents section, along with an “excludedDocuments” parameter with the recipient













Updated Code:
 
Figure 16 – Updated code to exclude recipient 2 from receiving one of the documents
 
Figure 17 – Success Message
Validation:
•	Replaced sample email with personal email address to test
 
Figure 18 – DocuSign envelope received to Barry Allen, containing both files
 
Figure 19 – DocuSign envelope received to Bruce Wayne, containing only specified file (and not both files)

 Possibility #2: Routing Order
•	Perhaps the author instead intended for Barry Allen to sign first, then Bruce Wayne to sign second.  
•	This can be accomplished via a defined routingOrder parameter.  
•	The code for this scenario is included in figure 16 but is commented out.

Further Enhancements/ Notes: 
•	Saved documentBase64 as a global variable to improve readability
•	Account -> Sending Settings -> Document Visibility cannot be turned to “Off”
o	https://admindemo.docusign.com/sending-settings




















API TEST 3

Create a successful API call to DocuSign that creates a single DocuSign envelope that combines the templates TestJSON1 and TestJSON2 and is directed to the following recipients: John Doe, johndoe@test.test and Jane Doe, janedoe@test.test.

Steps Taken:
•	Fed in raw JSON to Postman, “beautify”
•	Converted documentBase64 to variable
•	Converted to file using  https://base64.guru/converter/decode/file
o	Note: had to clean up using https://base64.guru/tools/repair
•	John Doe’s file:
 
Figure 20 - File contained within TestJSON1

•	Jane Doe’s file:
 
Figure 21 - File contained within TestJSON2
•	The desired data structure which combines multiple templates to one envelope is a “composite template” 
•	Task 1: upload JSON files as Templates within DocuSign 
 
Figure 22 – JSON Templated uploaded within DocuSign

•	Template 1 ID: 79cae74e-afe7-45c4-8e8f-63b94947f742
•	Template 2 ID: d6163c00-0a85-4554-8a87-98aa176972b2

•	Task 2: Create composite template
•	Adapt the format outlined here  with the requirements defined in the assignment
o	Note: we can ignore the “document” portion of the composite template as this is included in the serverTemplate template









Updated Code:
 
Figure 23 – Updated code to create an envelope of composite templates using pre-loaded JSON templates

 
Figure 24 – Success Message
Validation:
•	Replaced sample email with personal email addresses to test
 
Figure 25 – Email containing DocuSign envelope to John Doe

 
Figure 26 – Email containing DocuSign envelope to Jane Doe
 
Figure 27 – DocuSign envelope containing both documents

 
Figure 28 – DocuSign envelope containing both documents (continued)

Further Enhancements/ Notes: 
•	Error: "Freeform signing is not allowed for your account because it conflicts with other settings, please place signing tabs for each signer."
•	Fix:  set Document Visibility to “Off”
o	OR: add tab within JSON









Appendix

Step 1: Create Developer Account at: https://go.docusign.com/sandbox/productshot/

Step 2: Create an Application in Settings -> Apps and Keys -> Add App & Integration Key
Step 3: Generate secret key
 
Figure 29 – Creating an application

Step 4: copy/paste Integration Key and Secret Key, store locally
Step 5: define a call-back URL within the app (I used https://www.example.com/callback)

Step 6: Navigate to https://developers.docusign.com/tools/postman/
Step 7: Click “Set Up Your Environment” and copy/paste the Integration Key and Secret Key
 
Figure 30 – “Set Up Your Environment” page in https://developers.docusign.com/tools/postman/

Step 8: Click “eSignature REST API” -> Run in Postman
 
Figure 31 – Run in Postman page

Step 9: Notice the new folders imported into Postman.  Set upper right corner environment to DocuSign-account-d

Step 10: Navigate to URL:  https://account-d.docusign.com/oauth/auth?response_type=code&scope=signature%20impersonation&client_id={{iKey}&redirect_uri={callback}
Step 10: replace {{iKey}} with integration key, {callback} with callback URL specified in appicaiton (and steps 4 and 5 in this guide)
Step 11: Open a tab in your browser, copy/paste the URL.
Step 12: Copy/Paste the code from the updated URL in the browser
Step 13: Navigate to Postman DocuSign REST API Collection -> Authentication -> POST 01 Authorize Code Grant Access Token
Step 14: Place code in associated Key parameter
 
Figure 32 – Postman DocuSign REST API Collection -> Authentication -> POST 01 Authorize Code Grant Access Token -> Body

Step 15: Hit “Send” Note the corresponding access token
 
Figure 33 – Access Token visible

Step 16: Validate credentials with  DocuSign REST API Collection -> Authentication -> GET 04 Get User Info 
 
Figure 34 – Validate credentials

Step 17: Under “Headers” Section for each new request, create a new field “Authorization” with Value Bearer {{accessToken}}

 
Figure 35 – Headers per Request to ensure proper authentication


Reference:
•	https://developers.docusign.com/tools/postman/
•	https://www.youtube.com/watch?v=mV73U2tg9c0

