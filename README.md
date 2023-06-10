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

![image](https://github.com/awhouser213/TechAssess/assets/62894602/b571bcf4-04de-4396-abe4-a81d6eafa791)
Figure 2- Raw code for API TEST 1 before any edits, in Postman environment

Error 0: data not recognized as JSON
Step Taken: edit “header” field to contain Content-Type: application/json

Error 1:

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/3bc8a347-5d1b-4837-8bf9-96ae3941c24d)
Figure 3- First error message: invalid xPosition

Step Taken: removed the “-“ for recipient 2 xPosition
Note: may need to verify correct position with document designer

Error 2:

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/96ecb66c-4782-48d2-9948-0af97b9bdb0a)
Figure 4- Second error message: duplicate recipients

Steps Taken: changed recipientID from “1” to “2” for second recipient

Error 3:

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/86d71e1c-7eb2-4f37-9332-052c75f4dd04)
Figure 5- Third error message: Envelope is missing parameter

Step Taken: Added a new field below "emailSubject": "Simple Signing Example"    

Error 4:

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/fc327564-4901-4d8b-90b3-d0ad41c3bd39)
Figure 6- Fourth error message: DocumentID is incorrect

Steps Taken: First recipient contains a value for documentID of “2”, while documentID specified in documents section is “1”.  Updated recipient line to documentID 1.



Error 5:

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/a9ec536c-d81d-4df3-85d1-8c470610ab3e)
Figure 7- Fifth error message: Page number 2 does not exist

Steps Taken: The document itself is only 1 page (validated with base64 explorer site).  
Updated recipient 2-tab pagenumber from “2” to “1”

Error 6:

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/547acc78-b61b-4f1d-ae64-605e1c655907)
Figure 8- Sixth error message: invalid email address

Steps Taken: Added a valid email address to recipient 2











Updated Code:

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/0ad70faf-a04a-4313-95f6-85ae8133fbb8)
Figure 9- Updated code

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/e66c4200-d10c-4f5a-9dc1-c013dcf1dfc8)
Figure 10- Success message received in Body


Validation:
•	Replaced sample email with personal email address to test

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/60a87f4d-aa7a-4a14-a004-7c4af58a320f)
Figure 11 – Inbox containing DocuSign Envelope

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/1c97aef8-841f-47a2-a817-efda827cb65a)
Figure 12 – Document within DocuSign Envelope

Further Enhancements/ Notes: 
•	Saved documentBase64 as a global variable to improve readability


API TEST 2 
Attempt to make the API call in API TEST 2 successfully. Be prepared to explore this scenario in-depth during your technical interview.

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/7be5e02d-ea56-4fe6-a4ba-1340cce236c7)
Figure 13 – Raw code for API TEST 2 before any edits

Initial Observations:
•	Header is a POST request to DocuSign envelope 
o	POST requests are adding data to the server 
o	Body contents contain metadata about recipients/signers
o	Assumption: some form of contract is sent to two signatories


Steps Taken:
•	Created new Request in Postman, copy/pasted code as is and ran
•	Initial Run ran without an error message

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/1312f563-8aba-41c1-85e1-7a383e843706)
Figure 14 – Raw code for API TEST 2 in Postman


 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/31bf1f7e-99b9-4294-93bc-d56fb83bd51c)
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

![image](https://github.com/awhouser213/TechAssess/assets/62894602/8863bb99-203f-4a60-bcdc-e5138848277c)
Figure 16 – Updated code to exclude recipient 2 from receiving one of the documents

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/a748e7a0-499f-4c9e-bb38-b2d28a25e7fb)
Figure 17 – Success Message


Validation:
•	Replaced sample email with personal email address to test
 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/459e3ab4-9f9e-4c37-9dde-10ae01b29671)
Figure 18 – DocuSign envelope received to Barry Allen, containing both files


 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/8414287b-9b91-49ce-919b-c0426b6cdf4c)
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

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/04bf2264-05b3-4d38-a148-0340edb8c1c0)
Figure 20 - File contained within TestJSON1


•	Jane Doe’s file:
 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/3225c8bd-9db3-433b-90df-b23e145bee74)
Figure 21 - File contained within TestJSON2


•	The desired data structure which combines multiple templates to one envelope is a “composite template” 
•	Task 1: upload JSON files as Templates within DocuSign 

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/92d00349-106d-40db-9369-f7ac3fc27930)
Figure 22 – JSON Templated uploaded within DocuSign


•	Template 1 ID: 79cae74e-afe7-45c4-8e8f-63b94947f742
•	Template 2 ID: d6163c00-0a85-4554-8a87-98aa176972b2

•	Task 2: Create composite template
•	Adapt the format outlined here  with the requirements defined in the assignment
o	Note: we can ignore the “document” portion of the composite template as this is included in the serverTemplate template









Updated Code:
 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/4f61c675-bfe8-42aa-9948-f475ba972af1)
Figure 23 – Updated code to create an envelope of composite templates using pre-loaded JSON templates



 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/e7576ca6-2946-44e4-8aac-515c143cc93f)
Figure 24 – Success Message


Validation:
•	Replaced sample email with personal email addresses to test
 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/6cc9bab9-c07a-4736-9fce-6ab71ac8c0be)
Figure 25 – Email containing DocuSign envelope to John Doe


 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/4d9dcdc7-fff1-44cf-920f-e4aa1cdf522f)
Figure 26 – Email containing DocuSign envelope to Jane Doe


 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/d8647481-a0c8-43ef-8623-e418fa511d87)
Figure 27 – DocuSign envelope containing both documents


 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/c491b54b-ee98-447c-8360-7aaa10dd992a)
Figure 28 – DocuSign envelope containing both documents (continued)



Further Enhancements/ Notes: 
•	Error: "Freeform signing is not allowed for your account because it conflicts with other settings, please place signing tabs for each signer."
•	Fix:  set Document Visibility to “Off”
o	OR: add tab within JSON









Appendix

Step 1: Create Developer Account at: https://go.docusign.com/sandbox/productshot/

Step 2: Create an Application in Settings -> Apps and Keys -> Add App & Integration Key
Step 3: Generate secret key

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/8bf108fa-d890-41e8-8802-975075557161)
Figure 29 – Creating an application



Step 4: copy/paste Integration Key and Secret Key, store locally
Step 5: define a call-back URL within the app (I used https://www.example.com/callback)

Step 6: Navigate to https://developers.docusign.com/tools/postman/
Step 7: Click “Set Up Your Environment” and copy/paste the Integration Key and Secret Key

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/93635728-4593-415b-8a2d-c7d2ce751309)
Figure 30 – “Set Up Your Environment” page in https://developers.docusign.com/tools/postman/


Step 8: Click “eSignature REST API” -> Run in Postman
 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/40507c48-365e-451e-b1e3-cbb76baa02e1)
Figure 31 – Run in Postman page



Step 9: Notice the new folders imported into Postman.  Set upper right corner environment to DocuSign-account-d

Step 10: Navigate to URL:  https://account-d.docusign.com/oauth/auth?response_type=code&scope=signature%20impersonation&client_id={{iKey}&redirect_uri={callback}
Step 10: replace {{iKey}} with integration key, {callback} with callback URL specified in appicaiton (and steps 4 and 5 in this guide)
Step 11: Open a tab in your browser, copy/paste the URL.
Step 12: Copy/Paste the code from the updated URL in the browser
Step 13: Navigate to Postman DocuSign REST API Collection -> Authentication -> POST 01 Authorize Code Grant Access Token
Step 14: Place code in associated Key parameter

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/e099db54-be65-47b2-91de-9cad1122dace)
Figure 32 – Postman DocuSign REST API Collection -> Authentication -> POST 01 Authorize Code Grant Access Token -> Body



Step 15: Hit “Send” Note the corresponding access token
 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/465f83ea-a7f8-4be7-b9f2-e8aaeb7909e4)
Figure 33 – Access Token visible



Step 16: Validate credentials with  DocuSign REST API Collection -> Authentication -> GET 04 Get User Info 

 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/4f66c1fb-6f5f-48ef-9962-e758672c7304)
Figure 34 – Validate credentials



Step 17: Under “Headers” Section for each new request, create a new field “Authorization” with Value Bearer {{accessToken}}
 ![image](https://github.com/awhouser213/TechAssess/assets/62894602/4de85159-d6c8-4857-bf29-12b36edb1258)
Figure 35 – Headers per Request to ensure proper authentication



Reference:
•	https://developers.docusign.com/tools/postman/
•	https://www.youtube.com/watch?v=mV73U2tg9c0

