**NER variables to extract**
policy_num
email_id
enrolment_id
employee_id
corporate_name
policyholder_name

**Kafka interaction**:
Once the details required for policy coverage are extracted via NER and get live data, we place the ticket on kafka queue
If we failed to extract details then template will be sent and we wait for user to respond with complete details
Once a ticket is placed on kafka queue we update the database and wait for kafka to respond
-if we get the response from kafka 
  -it can send a response ,in this case we validate kafka's response against user query with help of another gpt call and if response is valid, the kafka response is slightly modified and sent to user and database is updated 
  -it can indicate of an error , in this case we unassign the ticket
-Every 30mins we run a cron that checks if there has been any tickets in database that are still waiting for kafka to respond , if such tickets are present and wait time of 30mins is exhausted then these tickets will be unassigned given that there has not been any modification to the ticket ( ex Agent didn't reply in the meanwhile etc) 