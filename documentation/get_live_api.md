## Overview 
Get live api is used to fetch other user details from one of the below details

The api can be hit with 
-enrolment id
-policy number
-email id
-employee number

We get multiple user related data with policy number , email id & employee number because multiple users can have same policy number , employee number and email id
- When there is multiple users , fuzzy search with name is used to find the correct member data

Sometimes users provide policy number in enrolment id field and enrolment id in policy number field , so if by hitting api 4 times with different payload , we couldn't locate the member then we hit the api with interchanged data and proceed with fuzzy search

For fuzzy search common middle names or surnames etc were removed to identify the user accurately 