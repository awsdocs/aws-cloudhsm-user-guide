# Managing HSM users with CloudHSM CLI<a name="manage-hsm-users-chsm-cli"></a>

Use [CloudHSM CLI](cloudhsm_cli-getting-started.md) command line tools to create and manage the users on your HSM with the latest SDK\.

**Topics**
+ [Understanding HSM users](#understanding-users)
+ [HSM user permissions table](#user-permissions-table-chsm-cli)
+ [Managing users](manage-chsm-cli-users.md)
+ [Managing MFA](login-mfa-token-sign.md)
+ [Managing M of N](quorum-auth-chsm-cli.md)

## Understanding HSM users<a name="understanding-users"></a>

 Most operations that you perform on the HSM require the credentials of an *HSM user*\. The HSM authenticates each HSM user and each HSM user has a *type* that determines which operations you can perform on the HSM as that user\. 

**Note**  
HSM users are distinct from IAM users\. IAM users who have the correct credentials can create HSMs by interacting with resources through the AWS API\. After the HSM is created, you must use HSM user credentials to authenticate operations on the HSM\.

**Topics**
+ [Unactivated admin](#unactivated-admin)
+ [Admin](#admin)
+ [Crypto user \(CU\)](#crypto-user-chsm-cli)
+ [Appliance user \(AU\)](#appliance-user-chsm-cli)

### Unactivated admin<a name="unactivated-admin"></a>

In CloudHSM CLI, The unactivated admin is a temporary user that exists only on the first HSM in an AWS CloudHSM cluster that has never been activated\. To [activate a cluster](activate-cluster.md), run the cluster activate command in CloudHSM CLI\. After running this command, unactivated admin are prompted to change the password\. After changing the password, the unactivated admin becomes an admin\. 

### Admin<a name="admin"></a>

In CloudHSM CLI, admin can perform user management operations\. For example, they can create and delete users and change user passwords\. For more information about admins, see the [HSM user permissions table](#user-permissions-table-chsm-cli)\. 

### Crypto user \(CU\)<a name="crypto-user-chsm-cli"></a>

A crypto user \(CU\) can perform the following key management and cryptographic operations\.
+ **Key management** – Create, delete, share, import, and export cryptographic keys\.
+ **Cryptographic operations** – Use cryptographic keys for encryption, decryption, signing, verifying, and more\.

For more information, see the [HSM user permissions table](#user-permissions-table-chsm-cli)\.

### Appliance user \(AU\)<a name="appliance-user-chsm-cli"></a>

The appliance user \(AU\) can perform cloning and synchronization operations on your cluster's HSMs\. AWS CloudHSM uses the AU to synchronize the HSMs in an AWS CloudHSM cluster\. The AU exists on all HSMs provided by AWS CloudHSM, and has limited permissions\. For more information, see the [HSM user permissions table](#user-permissions-table-chsm-cli)\.

AWS cannot perform any operations on your HSMs \. AWS cannot view or modify your users or keys and cannot perform any cryptographic operations using those keys\.

## HSM user permissions table<a name="user-permissions-table-chsm-cli"></a>

The following table lists HSM operations sorted by the type of HSM user or session that can perform the operation\.


|  | Admin | Crypto User \(CU\) | Appliance User \(AU\) | Unauthenticated Session | 
| --- | --- | --- | --- | --- | 
| Get basic cluster info¹ | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | 
| Change own password | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | Not applicable | 
| Change any user's password | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Add, remove users | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Get sync status² | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Extract, insert masked objects³ | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Key management functions⁴ | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Encrypt, decrypt | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Sign, verify | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Generate digests and HMACs | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
+  \[1\] Basic cluster information includes the number of HSMs in the cluster and each HSM's IP address, model, serial number, device ID, firmware ID, etc\. 
+  \[2\] The user can get a set of digests \(hashes\) that correspond to the keys on the HSM\. An application can compare these sets of digests to understand the synchronization status of HSMs in a cluster\. 
+  \[3\] Masked objects are keys that are encrypted before they leave the HSM\. They cannot be decrypted outside of the HSM\. They are only decrypted after they are inserted into an HSM that is in the same cluster as the HSM from which they were extracted\. An application can extract and insert masked objects to synchronize the HSMs in a cluster\. 
+  \[4\] Key management functions include creating, deleting, wrapping, unwrapping, and modifying the attributes of keys\. 