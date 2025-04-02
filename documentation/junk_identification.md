## Overview

The latest mail undergoes a junk first before conversation based processing 
Below check are applied to detect junk mails , if detected the junk mail is closed ( there is no feature of marking a ticket as junk in kapture , so currently they are just being closed)

**Subject check**
If the mail subject contains specific keywords then the ticket is closed Ex "limited offer", "car Dealership", "online courses"
**Body phrases check**
If the mail body contains specific phrases then the ticket is closed Ex phrase :"please do not reply to this e-mail
**Sender email check**
If sender is among junk email senders then the ticket is closed Ex junk sender: notifications@rediffenterprisepro.com
**Receiver email check**:
If user sends mail to preauth specific email address either in To or cc then the ticket is closed as junk
**Disposition based filters:**
   - For disposition Claim Documents | Accenture , if there are no attachments sent by user ticket is closed as junk
   - For disposition Claim Documents if To address doesn't have the folder's mail id it is closed as junk
**Gpt junk classification**
 - If gpt classifies a mail as junk based on promotional or spam keywords etc , then the ticket is closed as junk.