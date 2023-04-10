# To delete HSM users<a name="delete-user-cloudhsm-cli"></a>

Use user delete to delete a user\. You must log in as an admin to delete another user\.

**Tip**  
 You can't delete crypto users \(CU\) that own keys\. 

**To delete a user**

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

1. Use the login command and log in to the cluster as the admin\.

   ```
   aws-cloudhsm > login --username <username> --role admin
   ```

1. The system prompts you for your password\. Enter the password, and the output shows that the command was successful\.

   ```
   Enter password:
   {
     "error_code": 0,
     "data": {
       "username": "admin",
       "role": "admin"
     }
   }
   ```

1. Use the user delete command to delete the user\.

   ```
   aws-cloudhsm > user delete --username <username> --role <role>
   ```

For more information about user delete, see [deleteUser](cloudhsm_cli-user-delete.md)\.