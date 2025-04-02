**NER variables to extract**
policy_num,
email_id,
enrolment_id,
employee_id,
patient_name,
ailment_name,
hospital_name,
hospital_city,
hospital_state,
hospital_address,
hospital_pincode,
date_of_admission,
claim_amount


**Default Values**:
-if claim amount is empty we have default value set to 30000
-if patient name is empty then we take sender name as patient name

**Hospital details fetch**:
If hospital pincode is empty 
- apply fuzzy with state and city extracted from NER against the list of hospitals 
- apply fuzzy on hospital name 
-if address is present apply fuzzy with address


**DOA checks**
-For date of admission if we hit the api beyond a certain date range then the api fails saying intimation has to be within a date range
-To optimise on above flow , we check the date even before hitting the api and if it falls outside the date range , a template response is sent to indicate the user intimation is rejected because of date
- So when the date of admission is extracted successfully from the mail content, we check that date_of_intimation -5 <= doa < date_of_intimation+30 , if yes proceed further else send template . If user comes back on above template ,ticket is unassigned

**Cleaning and filtering values**:
 -dates are ensured to align with the format dd/mm/yyyy . Other formats like d/mm/yy or d-mm-yyyy etc are converted to dd/mm/yyyy format
-claim amount is filtered to include any punctuations like (',','.') etc . Rs ,lakhs ,thousands etc will be converted to reflect as zeroes in the claim amount Ex: if gpt fetched claim_amount: 'Rs 30k' , post filtering claim_amount will be '30000'.


**Reintimation**
- If the intimation api fails with below three errors 
   - Amount not sufficient for claim :
      -we get the balance amount out of the sum insured for the user and if it is not null , we hit the api again with this amount and mention in the response to user that the balance amount has been used for intimation
   - Incorrect Hospital details :
      -If hospital details are incorrect then we apply fuzzy on state,city, hospital name and address with the values present in the hospital list provided by backend . If we manage to extract hospital details using this process we rehit the api and send the appropriate response
   - Provided pincode is wrong
      -If pincode details are incorrect then the same process is followed as empty pincode and if extracted then we rehit the api with these details