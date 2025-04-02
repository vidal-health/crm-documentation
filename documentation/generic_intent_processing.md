## Overview

Once the intent is confirmed we proceed further with the processing of that particular intent
Each intent is processed slightly differently but the common parts are mentioned below

- **User Detail Retrieval:** Fetches essential user details like policy number, enrollment ID, etc., by supplying structured prompts along with the email content to GPT.
-**Demasking** The fetched ner details will be demasked using the demask key.
- **Cleaning and Filtering of fetched Details**. The fetched details will be cleaned and filtered so that they are of the right format and correctly extracted
- **User details fetch using api**. If required details are present there will a call to get live api - [get live api](/get_live_api.md) which return the other details of the user from already fetched details
-**Empty details but attachments present check**: If we failed to extract any details from the email content but note that there are attachments present in the email then we unassign the ticket because of the possibility that user has sent required details in attachments but we failed to extract them.
- **Required details check** If we have fetched all the details that are required to hit module specific apis, then the processing proceeds further to hit the apis and send appropriate response else if the details are incomplete a template based response will be sent by the system asking for the user to fill remaining details to process further.
- **API Integration:** Hits appropriate APIs if required fields are available. Responds based on the API outcomes.


Module specific processing:
-[intimation](/intent_processing/intimation.md)
-[policy coverage](/intent_processing/policy_coverage.md)
