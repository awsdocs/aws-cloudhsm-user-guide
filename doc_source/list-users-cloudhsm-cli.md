# To list all HSM users in the cluster<a name="list-users-cloudhsm-cli"></a>

 Use user list command to list all the users in the cluster\. You do not have to log in to run user list\. All user types can list users\. 

**Follow these steps to list all users in the cluster**

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

1. Enter the following command to list all the users in the cluster:

   ```
   aws-cloudhsm > user list
   ```

For more information about user list, see [user list](cloudhsm_cli-user-list.md)\.