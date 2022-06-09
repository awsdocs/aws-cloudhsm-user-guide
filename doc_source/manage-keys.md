# Managing keys in AWS CloudHSM<a name="manage-keys"></a>

In AWS CloudHSM, use any of the following to manage keys on the HSMs in your cluster: 
+ PKCS \#11 library
+ JCE provider
+ CNG and KSP providers
+ key\_mgmt\_util

Before you can manage keys, you must log in to the HSM with the user name and password of a crypto user \(CU\)\. Only a CU can create a key\. The CU who creates a key owns and manages that key\.

**Topics**
+ [Key synchronization in AWS CloudHSM](manage-key-sync.md)
+ [Using trusted keys to control key unwraps](cloudhsm_using_trusted_keys_control_key_wrap.md)
+ [AES key wrapping in AWS CloudHSM](manage-aes-key-wrapping.md)
+ [Using the command line to manage keys](using-kmu.md)