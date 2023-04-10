# To change HSM user passwords<a name="change-user-password-cloudhsm-cli"></a>

 Use the user change\-password command to change a password\. 

 User types and passwords are case sensitive, but user names are not case sensitive\.

 Admin, crypto user \(CU\), and appliance user \(AU\) can change their own password\. To change the password of another user, you must log in as an admin\. You cannot change the password of a user who is currently logged in\. 

**To change your own password**

1. Use the following command to start CloudHSM CLI interactive mode\.

------
#### [ Linux ]

   ```
   $ /opt/cloudhsm/bin/cloudhsm-cli interactive
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM\bin\> .\cloudhsm-cli.exe interactive
   ```

------

1. Use the login command and log in as the user with the password you want to change\.

   ```
   aws-cloudhsm > login --username <username> --role <role>
   ```

1. Enter the user's password\.

   ```
   Enter password:
   {
     "error_code": 0,
     "data": {
       "username": "admin1",
       "role": "admin"
     }
   }
   ```

1. Enter the user change\-password command\.

   ```
   aws-cloudhsm > user change-password --username <username> --role <role>
   ```

1. Enter the new password\.

1. Re\-enter the new password\.

**To change the password of another user**

1. Use the following command to start CloudHSM CLI interactive mode\.

------
#### [ Linux ]

   ```
   $ /opt/cloudhsm/bin/cloudhsm-cli interactive
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM\bin\> .\cloudhsm-cli.exe interactive
   ```

------

1. Use the login command and log in as an admin\.

   ```
   aws-cloudhsm > login --username <username> --role admin
   ```

1. Enter the admin's password\.

   ```
   Enter password:
   {
     "error_code": 0,
     "data": {
       "username": "admin1",
       "role": "admin"
     }
   }
   ```

1. Enter the user change\-password command along with the username of the user whose password you want to change\.

   ```
   aws-cloudhsm > user change-password --username <username> --role <role>
   ```

1. Enter the new password\.

1. Re\-enter the new password\.

For more information about user change\-password, see [user change\-password](cloudhsm_cli-user-change-password.md)\.