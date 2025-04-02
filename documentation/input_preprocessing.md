## Overview:

In a month we get average around 50000 tickets across 120+ dispositions.

Out of these tickets we are currently processing tickets of dispositions listed below (count 26):

    Queries,
    INTIMATION,
    BLR CRM,
    GI Gurgaon,
    HCL Intimation,
    General Queue,
    Amexrb,
    Claims Documents|Claims Documents-Others,
    Claims Documents|Claims Documents-BlrCorp,
    Claims Documents|Claims Documents-Accenture,
    Claims Documents|Claims Documents-HCL,
    Claims Documents|Claims Documents-Wipro,
    Claims Documents,
    Amexcl,
    AMEX India Support,
    Letters_Selffund,
    BpreauthProb,
    KPREAUTHPROB,
    Retail_Policies,
    ADECCO,
    ITCclaims,
    ITChepldesk,
    Emirates,
    BFShelpdesk,
    BFSclaims,
    Second Opinion

## Input Fetching:

Kapture provides 2 apis , pull ticket api and get ticket api
While the pull ticket api provides pending ticket ids of input dispositions between a date range  , the get ticket api provided the email conversations ,receivers, attachments etc. for a given ticket id.

Output parameter of use from pull ticket api:
ticketId,assignedToName,disposition,taskTitle,history

Output parameter of use from get ticket api:
sender_email,receiver,cc,email_body,attachments 

**Email Polling**

   - **Frequency and Volume:** Emails are fetched every 3 minutes by hitting pull ticket api, with each fetch capable of retrieving up to 500 tickets. Currently the upper limit to fetch is kept at 70 considering th average number of tickets we recieve.

   - **Filtering of these tickets:**: Each ticket is checked to see that
         - it doesn't have a flag which means AI cannot hanldle it ensuring that already processed tickets which was decided as cannot be completed by agent do not get picked and processed again
         - it doesn't have a flag which means AI started processing but there is no flag for AI ended processing ensuring that a ticket which has been picked in a previous poll and is still being processed doesn't get picked in the subsequent polling
         - It is also checked to see that the assigned to is either empty or AI Bot . ( An exception for a disposition named Letters Self fund is added where the assigne tp of these tickets can be the owner of the folder )
   - **Updating AI processing started flag:** If a ticket passes the above filters then we update the flag to say AI bot has started processing.
   - **Junk Email Check:** Emails will be checked if they come under junk category based on the mail ids,subject etc. If found as junk they will be marked as junk and stored in database without needing any further processing, else they will be placed on queue waiting for processing.
   - **Processing Capability:** Post filtering of all 70 tickets , the filtered tickets are places on a queue waiting to be processed further ,the system processes 5 tickets at a time, ensuring timely management of incoming emails.
   - **Concurrent Processing Check:** The system includes safeguards (mentioned above) to avoid fetching emails that are currently being processed, ensuring no duplication of work occurs.
   

 **Email Details Fetching**:

   - **Assign ticket to AI Bot**: Once a ticket is picked for processing , ,it is updated to reflect the assignee as AI bot so that agents don't assign it to themselves and can see AI is processing the ticket
   - **Other Email data fetching**: We hit a kapture get ticket api to fetch all the relevant email details for processing 
   Ex: No of conversations , email body across different conversations, history of ticket,attachments, senders, receivers, ccs etc    
        -**Note** that any bounce emails etc will not be considered part of number of conversations when we store the email details, also if rediff has sent the same mail consequently within a time gap then we ignore the duplicate mails while considering the valid conversations. 
    

 **Email Parsing**
   - **Content Preparation:** Initial parsing involves the removal of HTML tags , disclaimers in email body to clean up the email -  - Then based on the number of conversations , email content across different conversations is stored in a dictionary
   - **Junk Email Check:** After parsing, if the latest mail is external ,the system analyzes the email to determine if it is spam or junk.

 **Internal Check**
   - **Domain Verification:** The system checks if the email's domain is internal.
   
**Reading Attachments**:
 - The attachments present in the mail are checked to see if they are readable and the content of readable attachments is extracted and appended to the email content .
 -If there are any compressed files in attachments or there are more than one non extractable (jpeg) attachments then the ticket is unassigned.

 **Masking**: The email content which will be sent to gpt for classification and NER etc will be masked before making the OPENAI api call ensuring the privacy of user data 
 [masking][documentation/maskig.md]