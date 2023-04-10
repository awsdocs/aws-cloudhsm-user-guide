# Change the quorum minimum value for admins<a name="quorum-auth-chsm-cli-min-value"></a>

After you [set the quorum minimum value](quorum-auth-chsm-cli-first-time.md#quorum-admin-set-quorum-minimum-value-chsm-cli) so [admins](manage-hsm-users-chsm-cli.md#admin) can use quorum authentication, you might want to change the quorum minimum value\. The HSM allows you to change the quorum minimum value only when the number of approvers is the same or higher than the current quorum minimum value\. For example, if the quorum minimum value is two \(2\), at least two \(2\) admins must approve to change the quorum minimum value\.

To get quorum approval to change the quorum minimum value, you need a *quorum token* for the quorum service using the quorum token\-sign set\-quorum\-value command\. To generate a quorum token for the for the quorum service using the quorum token\-sign set\-quorum\-value command, the quorum service must be higher than one \(1\)\. This means that before you can change the quorum minimum value for *user service*, you might need to change the quorum minimum value for *quorum service*\.

The following table lists the HSM service identifiers along with their names, descriptions, and the commands that are included in each service\.

**To change the quorum minimum value for admins**

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
   aws-cloudhsm>login --username <admin> --role admin
   ```

1. Use the quorum token\-sign list\-quorum\-values command to get the quorum minimum values for all service names\. For more information, see the example below\.

1. If the quorum minimum value for quorum service is lower than the value for user service, use the quorum token\-sign set\-quorum\-value command to change the value for *quorum service*\. Change the value for quorum service to one \(1\) that is the same or higher than the value for user service\. For more information, see the following example\.

1. [Generate a quorum token](quorum-auth-chsm-cli-admin.md#quorum-admin-gen-token-chsm-cli), taking care to specify quorum service as the service for which you can use the token\.

1. [Get approvals \(signatures\) from other admins](quorum-auth-chsm-cli-admin.md#quorum-admin-get-approval-signatures-chsm-cli)\.

1. [Approve the token on the AWS CloudHSM cluster and execute a user management operation\.](quorum-auth-chsm-cli-admin.md#quorum-admin-approve-token-chsm-cli)\. 

1. Use the quorum token\-sign set\-quorum\-value command to change quorum minimum value for user service\.

**Example – Get quorum minimum values and change the value for *quorum service***  
The following example command shows that the quorum minimum value for *user service* is currently two \(2\)\.  

```
aws-cloudhsm > quorum token-sign list-quorum-values{
  "error_code": 0,
  "data": {
    "user": 2,
    "quorum": 1
  }
}
```
To change the quorum minimum value for *quorum service*, use the quorum token\-sign set\-quorum\-value command, setting a value that is the same or higher than the value for *user service*\. The following example sets the quorum minimum value for *quorum service* to two \(2\), the same value that is set for *user service*\.  

```
aws-cloudhsm > quorum token-sign set-quorum-value --service quorum --value 2{
  "error_code": 0,
  "data": "Set quorum value successful"
}
```
The following command shows that the quorum minimum value is now two \(2\) for *user service* and *quorum service*\.  

```
aws-cloudhsm > quorum token-sign list-quorum-values{
  "error_code": 0,
  "data": {
    "user": 2,
    "quorum": 2
  }
}
```