# Migrate from Client SDK 3 to Client SDK 5<a name="migrate-sdk"></a>

Use this table to understand how to get started with Client SDK 5\.


****  

| If you did this with Client SDK 3 | You do this in Client SDK 5 | 
| --- | --- | 
| Manage HSM users with CloudHSM Management Utility \(CMU\)\. | To manage HSM users with Client SDK 5, you must use a standalone version of CMU\. For more information, see [Download CMU](cli-users.md#get-cli-users) and [Manage HSM users with CMU](cli-users.md#manage-users)\. | 
| Run a single HSM cluster\. | To run a single HSM cluster with Client SDK 5, you must first manage client key durability settings by setting disable\_key\_availability\_check to True\. For more information, see [Key Synchronization](manage-key-sync.md) and [Client SDK 5 Configure Tool](configure-sdk-5.md)\. | 
| Use Oracle Database Transparent data Encryption\. | To use Oracle Database Transparent data Encryption, you must continue to use Client SDK 3\. For more information, see [Oracle Database Transparent data Encryption](oracle-tde.md)\. | 
| Use the same key handles across different runs of an application\. | To successfully use key handles in Client SDK 5, you must obtain key handles each time you run an application\. If you have existing applications that expect to use the same key handles across different runs, you must modify your code to obtain the key handle each time you run the application\. This change is in compliance with the [PKCS \#11 2\.40 specification](http://docs.oasis-open.org/pkcs11/pkcs11-base/v2.40/os/pkcs11-base-v2.40-os.html#_Toc416959689)\. | 