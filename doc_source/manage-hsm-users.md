# Managing HSM users in AWS CloudHSM<a name="manage-hsm-users"></a>

 In AWS CloudHSM, you must use [CloudHSM Management Utility](cloudhsm_mgmt_util-getting-started.md) \(CMU\), a command line tool to create and manage the users on your HSM\. You can also setup quorum authentication\. For more information about the types of users available and what actions those users can perform, see [Understanding HSM users](#understanding-users)\.

**Topics**
+ [Understanding HSM users](#understanding-users)
+ [HSM user permissions table](#user-permissions-table)
+ [Using CMU to manage users](cli-users.md)
+ [Managing 2FA](manage-2fa.md)
+ [Managing quorum authentication](quorum-authentication.md)

## Understanding HSM users<a name="understanding-users"></a>

 Most operations that you perform on the HSM require the credentials of an *HSM user*\. The HSM authenticates each HSM user and each HSM user has a *type* that determines which operations you can perform on the HSM as that user\. 

**Topics**
+ [Precrypto officer \(PRECO\)](#preco)
+ [Crypto officer \(CO \| PCO\)](#crypto-officer)
+ [Crypto user \(CU\)](#crypto-user)
+ [Appliance user \(AU\)](#appliance-user)

### Precrypto officer \(PRECO\)<a name="preco"></a>

The precrypto officer \(PRECO\) is a temporary user that exists only on the first HSM in an AWS CloudHSM cluster\. The first HSM in a new cluster contains a PRECO user with a default user name and password\. The PRECO user can only change its own password and perform read\-only operations on the HSM\. You use the PRECO user to activate a cluster\. To [activate a cluster](activate-cluster.md), you log in to the HSM and change the PRECO user's password\. When you change the password, the PRECO user becomes the primary crypto officer \(PCO\)\. 

### Crypto officer \(CO \| PCO\)<a name="crypto-officer"></a>

A crypto officer \(CO\) can perform user management operations\. For example, a CO can create and delete users and change user passwords\. PCO is the designation for first CO you create, the primary CO\. For more information about CO users, see the [HSM user permissions table](#user-permissions-table)\. When you [activate a new cluster](activate-cluster.md), the user changes from a [Precrypto Officer](#preco) \(PRECO\) to a crypto officer \(CO\)\. 

### Crypto user \(CU\)<a name="crypto-user"></a>

A crypto user \(CU\) can perform the following key management and cryptographic operations\.
+ **Key management** – Create, delete, share, import, and export cryptographic keys\.
+ **Cryptographic operations** – Use cryptographic keys for encryption, decryption, signing, verifying, and more\.

For more information, see the [HSM user permissions table](#user-permissions-table)\.

### Appliance user \(AU\)<a name="appliance-user"></a>

The appliance user \(AU\) can perform cloning and synchronization operations\. AWS CloudHSM uses the AU to synchronize the HSMs in an AWS CloudHSM cluster\. The AU exists on all HSMs provided by AWS CloudHSM, and has limited permissions\. For more information, see the [HSM user permissions table](#user-permissions-table)\.

AWS uses the AU to perform cloning and synchronization operations on your cluster's HSMs\. AWS cannot perform any operations on your HSMs except those granted to the AU and unauthenticated users\. AWS cannot view or modify your users or keys and cannot perform any cryptographic operations using those keys\.

## HSM user permissions table<a name="user-permissions-table"></a>

The following table lists HSM operations sorted by the type of HSM user or session that can perform the operation\.


|  | Crypto Officer \(CO\) | Crypto User \(CU\) | Appliance User \(AU\) | Unauthenticated Session | 
| --- | --- | --- | --- | --- | 
| Get basic cluster info¹ | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | 
| Zeroize an HSM² | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | 
| Change own password | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | Not applicable | 
| Change any user's password | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Add, remove users | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Get sync status³ | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Extract, insert masked objects⁴ | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Key management functions⁵ | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Encrypt, decrypt | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Sign, verify | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
| Generate digests and HMACs | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-yes.png) Yes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/icon-no.png) No | 
+  \[1\] Basic cluster information includes the number of HSMs in the cluster and each HSM's IP address, model, serial number, device ID, firmware ID, etc\. 
+  \[2\] When an HSM is zeroized, all keys, certificates, and other data on the HSM is destroyed\. You can use your cluster's security group to prevent an unauthenticated user from zeroizing your HSM\. For more information, see [Create a cluster](create-cluster.md)\. 
+  \[3\] The user can get a set of digests \(hashes\) that correspond to the keys on the HSM\. An application can compare these sets of digests to understand the synchronization status of HSMs in a cluster\. 
+  \[4\] Masked objects are keys that are encrypted before they leave the HSM\. They cannot be decrypted outside of the HSM\. They are only decrypted after they are inserted into an HSM that is in the same cluster as the HSM from which they were extracted\. An application can extract and insert masked objects to synchronize the HSMs in a cluster\. 
+  \[5\] Key management functions include creating, deleting, wrapping, unwrapping, and modifying the attributes of keys\. 