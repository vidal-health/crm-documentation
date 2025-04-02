Categories and subcategories defined:

CLAIMS MANAGEMENT
- Claim Status & Modifications
- Claim Document Submission
- Claim Submission Procedures 
- Claim Payments and Discrepancies
- Claim Rejection Follow Up
- Claim Cancellation
- Others

ECARD & MEMBERSHIP MANAGEMENT
- Ecard Request
- Ecard Updates

INTIMATIONS
- Intimation Submission
- Others

POLICY MANAGEMENT
- Policy Updates
- Policy Coverage
- Policy Number/Documents Request
- General Policy Queries

PORTAL ISSUES
- Information Access
- Login & Access Issues
- Technical Errors and Bugs
- Profile Management
- Others

PRE-AUTHORIZATION/CASHLESS
- Pre-Authorization Requests
- Pre-Authorization Modifications
- Others

SHORTFALL
- Shortfall Document Submission
- Shortfall Follow-ups
- Others

MULTIPLE QUERIES/REQUESTS
- Multiple Queries/Requests

FINAL BILLING & DISCHARGE
- Billing & Discharge Submission


 
**Category Classification:**
- **Dual AI Models:** Utilizes both GPT and BERT models for accurate classification, depending on the folder's automation mapping.
- **Dynamic prompt for GPT**: Based on a config ,the prompt passed to GPT will be generated dynamically to only include categories which are in the scope for that disposition
     Ex: Intimation disposition has only intimation submission category in its scope . So in the prompt the only categories passed will be intimation submission and others.
- **Inhouse models**: Multiple Distillbert model are trained inhouse on above categories (1 vs all) and subcategories , the top most category bsaed on the confidence is considered as bert category
- **Model Selection:** If the disposition is within automation parameters, both models are used. If not, only BERT is used (meaning GPT is not applicable or 'NA' for some dispositions).
- **Final Category Decision**- If the classifications from GPT and BERT match, or if only BERT was used for classification (GPT is 'NA') then the bert category is the final category
- **Exceptions**:   
            Above holds true for all categories except Claim Management and Shortfall. If GPT and Bert say either of these and they don't match then there is check for keywords related to shortfall in the mail content. If found then we proceed with the subcategory classification for shortfall module else the mail/ticket will be left for Agent to handle

 **Subcategory Classification**

   -  Once the category is decided , both Gpt and bert will be used for Subcategory classification if the disposition is in folder's automation mapping else only bert will be used for subcategory classification.
    If they match then the intent is confirmed and intent based processing starts only if this intent is part of that folders automation scope,else the ticket is unassigned .
    If the disposition in not in folder automation mapping or the matched intent is not in folder automation mapping then the ticket is unassigned.
      -**Exception**: Only GPT is being used for Ecard subcategory classification.
 