# CRM Email Automation System README

## Overview
This CRM Email Automation System leverages advanced artificial intelligence technologies to enhance customer service operations. It automates the processing of customer emails, streamlining the handling of inquiries and requests by categorizing, fetching relevant details, and responding appropriately based on predefined rules and AI insights.

## Key Components

### Input Processing
- [Input fetching and preprocessing](documentation/input_preprocessing.md)
  - Email polling and fetching
  - Email parsing and preprocessing 
  - Internal detection
  - Attachment handling
  - Data masking

### Classification Components
- [Category and subcategory finalisation](documentation/classification.md)
- GPT and Bert classification
-If matched GPT and Bert Subclassification

### Intent Processing Components
- User Detail Extraction based on intent
- Data Cleaning & Filtering
- API Integration

- [General Process](documentation/generic_intent_processing.md)



## System Flow



### System Flow Diagram
![flow](image.png)


   ↓



 **Detailed Ticket Processing**
   - **Matching Criteria:** Processes the ticket further when the subcategory from GPT matches BERT's, and the category is included in the folder automation mapping.
   - **User Detail Retrieval:** Fetches essential user details like policy number, enrollment ID, etc., by supplying structured prompts along with the email content to GPT.
   - **Cleaning and Filtering of fetched Details**. The fetched details will be cleaned and filtered so that they are of the right format and correctly extracted
   - **User details fetch using api**. If required details are present there will a call to specific api's which return the other details of the user from already fetched details
   - **Required details check** If we have fetched all the details that are required to hit module specific apis, then the processing proceeds further to hit the apis and send appropriate response else if the details are incomplete a template based response will be sent by the system asking for the user to fill remaining details to process further.
   - **API Integration:** Hits appropriate APIs if required fields are available. Responds based on the API outcomes.
   - **Response Handling:** If a user responds to a templated request with complete details, the system processes those details, hits the necessary APIs, and sends a final response. If unable to fetch details after a templated request, or if API interaction fails, the ticket is unassigned.

## Detailed Conversation Flow
The CRM Email Automation System handles email interactions based on the conversation history to ensure contextual and relevant responses. Here’s how the system manages email tickets across different conversation counts:

- **Conversation 1 (Convo = 1)**
  - **Initial Interaction:** When the assignee is "AI_BOT" and it is the user's first contact:
  - **Process Flow:** The system to the standard processing protocol:
    - **Email Parsing:** Strips HTML tags to clean the email content.
    - **Junk Email Check:** Assesses whether the email is spam.
    - **Content Length Check:** Summarizes the content if it exceeds a certain length, optimizing it for AI classification.
    - **Read Attachments**: Reads the attachments and appends the extracted text.
    - **Internal,keywords check**: Mail is checked to see it is not internal and specific keywords are not present
    - **Masking** : Mail is masked and demask key is stored .
    - **Ticket Processing:** Classifies the ticket using AI models based on category and subcategory.
    - **Detail Retrieval:** After clearing all filters, the system uses Named Entity Recognition (NER) via GPT to extract preliminary user details. Following this, it invokes the `getActiveLiveDetails` function to gather all remaining user information based on parameters obtained by NER, such as combining policy number with fuzzy name matching, or employee ID with name, to ensure a thorough data collection process.
    - **API Integration and User Re-engagement:** With complete details, the system engages necessary APIs for further action. In cases where essential information is missing, it issues an "Insufficient Details Table Template" to the user, requesting the required data and facilitating effective follow-up upon user reply.

- **Conversation 2 (Convo = 2)**
  - **Follow-Up Interaction:** Occurs when the same user sends another email:
  - **Internal Review:** If the domain is internal, the system presumes agent intervention and closes the ticket.
  - **External Follow-Up:** If the domain is external then we proceed to unassign the ticket because of back to back emails from same user.

- **Conversation 3 (Convo = 3)**
  - **Subsequent Inquiry:** Engages when the user replies following an initial API interaction or after receiving a templated request:
  - **Reply to template ai response:** If the response has all template mandatory details filled ,then we filter the details and hit api accordingly. If the details are incomplete after filtering or the user itself hadn't sent the complete details then the ticket is unassigned
  - **Positive Feedback for successful ai response:** If the response is affirmative, such as "thank you," the system closes the ticket.
  - **Further Queries:** If  user seeks additional information or clarification, the system unassigns the ticket

- **Conversation 4 (Convo = 4)**
  - **Internal Responses:** If the email originates from an internal source, the ticket is closed under the assumption of resolution or internal management.
  - **External Persistence:** In the case of ongoing external queries:
    -- **Indicative of Dissatisfaction or Complexity:** -The system may unassign the ticket due to its complexity or the need for specialized attention.
- **Conversation 5 (Convo = 5)**
  - **Continued Follow-Up:**
  - **Handling Non-Internal Emails:** The ticket is handled if it is of external domain in 5th conversation,
 - **Positive Feedback for successful ai response:** If the response is affirmative, such as "thank you," the system closes the ticket.
  - **Further Queries:** If  user seeks additional information or clarification, the system unassigns the ticket
    
## Conclusion
The conversation-based approach of the CRM Email Automation System ensures that each customer interaction is handled with attention to detail and contextual awareness. By leveraging advanced AI classification and detailed process checks, the system effectively manages customer interactions across multiple touchpoints, enhancing overall customer satisfaction and operational efficiency.
