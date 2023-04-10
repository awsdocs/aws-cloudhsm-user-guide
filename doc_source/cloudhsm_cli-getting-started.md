# Getting started with CloudHSM Command Line Interface \(CLI\)<a name="cloudhsm_cli-getting-started"></a>

CloudHSM Command Line Interface \(CLI\) allows you to manage users in your AWS CloudHSM cluster\. Use this topic to get started with basic HSM user management tasks, such as creating users, listing users, and connecting CloudHSM CLI to the cluster\.

1. To use CloudHSM CLI, you must first run the configure script with the IP address of your HSM\.

------
#### [ Linux ]

   ```
   $ sudo /opt/cloudhsm/bin/configure-cli -a <IP address>
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM> .\configure-cli.exe -a <IP address>
   ```

------

1. Use the following command to start CloudHSM CLI\.

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

1. Use the login command to log in to the cluster\. All users can use this command\.

   The command in the following example logs in *admin*, which is the default [admin](manage-hsm-users-chsm-cli.md#understanding-users) account\. You set this user's password when you [activated the cluster](activate-cluster.md)\.

   ```
   aws-cloudhsm > login --username admin --role admin
   ```

   The system prompts you for your password\. You enter the password, and the output shows that the command was successful\.

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

1. Run the user list command to list all the users on the cluster\.

   ```
   aws-cloudhsm > user list
   {
     "error_code": 0,
     "data": {
       "users": [
         {
           "username": "admin",
           "role": "admin",
           "locked": "false",
           "mfa": [],
           "cluster-coverage": "full"
         },
         {
           "username": "app_user",
           "role": "internal(APPLIANCE_USER)",
           "locked": "false",
           "mfa": [],
           "cluster-coverage": "full"
         }
       ]
     }
   }
   ```

1.  Use user create to create a CU user named **example\_user**\. 

   You can create CUs because in a previous step you logged in as an admin user\. Only admin users can perform user management tasks, such as creating and deleting users and changing the passwords of other users\. 

   ```
   aws-cloudhsm > user create --username example_user --role crypto-user     
   Enter password:
   Confirm password:
   {
    "error_code": 0,
    "data": {
      "username": "example_user",
      "role": "crypto-user"
    }
   }
   ```

1.  Use user list to list all the users on the cluster\. 

   ```
   aws-cloudhsm > user list
   {
     "error_code": 0,
     "data": {
       "users": [
         {
           "username": "admin",
           "role": "admin",
           "locked": "false",
           "mfa": [],
           "cluster-coverage": "full"
         },
         {
           "username": "example_user",
           "role": "crypto_user",
           "locked": "false",
           "mfa": [],
           "cluster-coverage": "full"
         },
         {
           "username": "app_user",
           "role": "internal(APPLIANCE_USER)",
           "locked": "false",
           "mfa": [],
           "cluster-coverage": "full"
         }
       ]
     }
   }
   ```

1. Use the logout command to log out of AWS CloudHSM cluster\.

   ```
   aws-cloudhsm > logout
   {
     "error_code": 0,
     "data": "Logout successful"
   }
   ```

1. Use the quit command to stop the CLI\.

   ```
   aws-cloudhsm > quit
   ```