# Reference for CloudHSM CLI commands<a name="cloudhsm_cli-reference"></a>

CloudHSM CLI helps admins manage users in their AWS CloudHSM cluster\.

For a quick start, see [Getting started with CloudHSM Command Line Interface \(CLI\)](cloudhsm_cli-getting-started.md)\. 

 CloudHSM CLI can be run in two modes: Interactive Mode and Single Command Mode\. 

The following topics describe commands in CloudHSM CLI\. 


| Command | Description | User Type | 
| --- | --- | --- | 
| [cluster activate](cloudhsm_cli-cluster-activate.md) | Activates an CloudHSM cluster and provides confirmation the cluster is new\. This must be done before any other operations can be performed\. | Unactivated admin | 
| [login](cloudhsm_cli-login.md) | Log in to your AWS CloudHSM cluster\. | Admin, crypto user \(CU\), and appliance user \(AU\) | 
| [logout](cloudhsm_cli-logout.md) | Log out of your AWS CloudHSM cluster\. | Admin, CU, and appliance user \(AU\) | 
| [user change\-mfa](cloudhsm_cli-user-change-mfa.md) | Changes a user's multi\-factor authentication \(MFA\) strategy\. | Admin, CU | 
| [user change\-password](cloudhsm_cli-user-change-password.md) | Changes the passwords of users on the HSMs\. Any user can change their own password\. Admins can change anyone's password\. | Admin, CU | 
| [user create](cloudhsm_cli-user-create.md) | Creates a user in your AWS CloudHSM cluster\. | Admin | 
| [user delete](cloudhsm_cli-user-delete.md) | Deletes a user in your AWS CloudHSM cluster\. | Admin | 
| [user list](cloudhsm_cli-user-list.md) | Lists the users in your AWS CloudHSM cluster\. | All [1](#cli-ref-1), including unauthenticated users\. Login is not required\. | 
| [user change\-quorum token\-sign register](cloudhsm_cli-user-chqm-token-reg.md) | Registers the quorum token\-sign quorum strategy for a user\. | Admin | 
| [quorum token\-sign delete](cloudhsm_cli-qm-token-del.md) | Deletes one or more tokens for a quorum authorized service\. | Admin | 
| [quorum token\-sign generate](cloudhsm_cli-qm-token-gen.md) | Generates a token for a quorum authorized service\. | Admin | 
| [quorum token\-sign list](cloudhsm_cli-qm-token-list.md) | Lists all token\-sign quorum tokens present in your CloudHSM cluster\. | All [1](#cli-ref-1), including unauthenticated users\. Login is not required\. | 
| [Quorum token\-sign list\-quorum\-values](cloudhsm_cli-qm-token-list-qm.md) | Lists the quorum values set in your CloudHSM cluster\. | All [1](#cli-ref-1), including unauthenticated users\. Login is not required\. | 
| [quorum token\-sign list\-timeouts](cloudhsm_cli-qm-token-list-tm.md) | Obtains the token timeout period in seconds for all token types\. | Admin and crypto user | 
| [quorum token\-sign set\-quorum\-value](cloudhsm_cli-qm-token-set-qm.md) | Sets a new quorum value for a quorum authorized service\. | Admin | 
| [quorum token\-sign set\-timeout](cloudhsm_cli-qm-token-set-tm.md) | Sets the token timeout period in seconds for each token type\. | Admin | 

**Annotations**
+ \[1\] All users includes all listed roles and users not logged in\.