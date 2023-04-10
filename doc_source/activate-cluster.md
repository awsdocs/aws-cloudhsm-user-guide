# Activate the cluster<a name="activate-cluster"></a>

When you activate an AWS CloudHSM cluster, the cluster's state changes from initialized to active\. You can then [manage the hardware security module \(HSM\) users](manage-hsm-users.md) and [use the HSM](use-hsm.md)\. 

**Important**  
Before you can activate the cluster, you must first copy the issuing certificate to the default location for the platform on each EC2 instance that connects to the cluster \(you create the issuing certificate when you initialize the cluster\)\.  
Linux  

```
/opt/cloudhsm/etc/customerCA.crt
```
Windows  

```
C:\ProgramData\Amazon\CloudHSM\customerCA.crt
```

After placing the issuing certificate, install CloudHSM CLI and run the [cluster activate](cloudhsm_cli-cluster-activate.md) command on your first HSM\. You will notice the admin account on the first HSM in your cluster has the [unactivated\-admin](manage-hsm-users-chsm-cli.md#understanding-users) role\. This a temporary role that only exists prior to cluster activation\. When you activate your cluster, the unactivated\-admin role changes to admin\.

**Activate a cluster**

1. Connect to the client instance that you previously launched in\. For more information, see [Launch an Amazon EC2 client instance](launch-client-instance.md)\. You can launch a Linux instance or a Windows Server\. 

1. Run the CloudHSM CLI in interactive mode\.

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

1. \(Optional\) Use the user list command to display the existing users\.

   ```
   aws-cloudhsm >  user list
   {
     "error_code": 0,
     "data": {
       "users": [
         {
           "username": "admin",
           "role": "unactivated-admin",
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

1. Use the cluster activate command to set the initial admin password\.

   ```
   aws-cloudhsm >  cluster activate
   
    Enter password:<NewPassword>
    Confirm password:<NewPassword>
   
   {
     "error_code": 0,
     "data": "Cluster activation successful"
   }
   ```

   We recommend that you write down the new password on a password worksheet\. Do not lose the worksheet\. We recommend that you print a copy of the password worksheet, use it to record your critical HSM passwords, and then store it in a secure place\. We also recommended that you store a copy of this worksheet in secure off\-site storage\. 

1. \(Optional\) Use the user list command to verify that the user's type changed to [admin/CO](manage-hsm-users-cmu.md#crypto-officer)\. 

   ```
   aws-cloudhsm >  user list
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

1. Use the quit command to stop the CloudHSM CLI tool\.

   ```
   aws-cloudhsm > quit
   ```

For more information about working with CloudHSM CLI or the CMU, see [Understanding HSM Users](manage-hsm-users-chsm-cli.md#understanding-users) and [Understanding HSM User Management with CMU](cli-users.md#understand-users)\.