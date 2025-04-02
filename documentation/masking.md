## Overview
The email content of the input mail will be masked to protect the privacy of user
Masking involved masking of corporates , any numbers mentioned ,email ids
Above masking will ensure that is impossible to map to any user for 3rd party organisations

- **Corporates**: All the corporates are assigned a unique id with which it will be replaced, ex Wipro will be masked as Xguav etc
- **Numbers**: Numbers are converted to strings and each character is replaced with a equivalent number of value less than the number
 Ex: 548 can be masked as 432. No 2 numbers will be masked with the same number , Ex : if 548 is masked bu 432 , then other numbers cannot be masked by 432 
- **Emails**: Emails are recognised in the input email content and replaced by email ids like test_1@gmail.com etc

For demasking the hash maps with masked_value and maskedby_values as key value pairs are stored and used later on.
