# Managing two\-factor authentication \(2FA\) for crypto officers<a name="manage-2fa"></a>

For increased security, you can configure two\-factor authentication \(2FA\) to help protect the cluster\. You can only enable 2FA for crypto officers \(CO\)\. 

**Note**  
You cannot enable 2FA for crypto users \(CU\) or applications\. Two\-factor authentication \(2FA\) is only for CO users\.

**Topics**
+ [Understanding 2FA](#understand-2fa)
+ [Working with 2FA](#enable-2fa)

## Understanding 2FA for HSM users<a name="understand-2fa"></a>

When you log in to a cluster with a 2FA\-enabled hardware service module \(HSM\) account, you provide cloudhsm\_mgmt\_util \(CMU\) with your password—the first factor, what you know—and CMU provides you with a token and prompts you to have the token signed\. To provide the second factor—what you have—you sign the token with a private key from a key pair you've already created and associated with the HSM user\. To access the cluster, you provide the signed token to CMU\.

### Quorum authentication and 2FA<a name="quorum-2fa"></a>

The cluster uses the same key for quorum authentication and for 2FA\. This means a user with 2FA enabled is effectively registered for M\-of\-N\-access\-control \(MofN\)\. To successfully use 2FA and quorum authentication for the same HSM user, consider the following points:
+ If you are using quorum authentication for a user today, you should use the same key pair you created for the quorum user to enable 2FA for the user\. 
+ If you add the 2FA requirement for a non\-2FA user that is not a quorum authentication user, then you register that user as an MofN user with 2FA authentication\.
+ If you remove the 2FA requirement or change the password for a 2FA user that is also a quorum authentication user, you will also remove the registration of the quorum user as an MofN user\.
+ If you remove the 2FA requirement or change the password for a 2FA user that is also a quorum authentication user, but you *still want that user to participate in quorum authentication*, then you must register that user again as an MofN user\.

For more information about quorum authentication, see [Managing quorum authentication](quorum-authentication.md)\.

## Working with 2FA for HSM users<a name="enable-2fa"></a>

This section describes how to work with 2FA for HSM users, including creating 2FA HSM users, rotating keys, and logging in to the HSM as 2FA\-enabled users\. For more information about working with HSM users, see [Managing HSM users in AWS CloudHSM](manage-hsm-users.md), [Using CloudHSM Management Utility \(CMU\) to manage users](cli-users.md), [createUser](cloudhsm_mgmt_util-createUser.md), [loginHSM and logoutHSM](cloudhsm_mgmt_util-loginLogout.md), and [changePswd](cloudhsm_mgmt_util-changePswd.md)\.

### Creating 2FA users<a name="create-2fa"></a>

To enable 2FA for an HSM user, use a key that meets the following requirements\. 

#### 2FA key pair requirements<a name="enable-2fa-kms"></a>

You can create a new key pair or use an existing key that meets the following requirements\. 
+ Key type: Asymmetric
+ Key usage: Sign and Verify
+ Key spec: RSA\_2048
+ Signing algorithm includes: 
  + `sha256WithRSAEncryption`

**Note**  
If you are using quorum authentication or plan to use quorum authentication, see [Quorum authentication and 2FA](#quorum-2fa)\.

You use CMU and the key pair to create a new CO user with 2FA enabled\.

**To create CO users with 2FA enabled**

1. Use CMU to log in to the HSM as a CO\.

1.  Use createUser to create a CO user with 2FA enabled\. Use the `-2fa` parameter and specify a location in the file system for the system to write the `authdata` file\. This file will include a digest for each HSM in the cluster\.

   ```
   aws-cloudhsm>createUser CO example-user <password> -2fa /path/to/authdata
   ```

   CMU prompts you to use the private key to sign the digests in the `authdata` file and return the signatures with the public key\.

1. Use the private key to sign the digests in the `authdata` file, add the signatures and the public key to the JSON formatted `authdata` file, and then provide CMU with the location of the `authdata` file\. For more information, see [Configuration reference](#reference-2fa)\.

### Logging in 2FA users<a name="login-2fa"></a>

**To log in as CO users with 2FA enabled**

1. Use CMU to log in to the HSM as a CO with 2FA enabled\.
**Note**  
 To log in as a CO user with 2FA, you must first enable 2FA for the CO user\. If you specify the `-2fa` parameter for a non\-2FA user, the system prompts you as if you were logging in as a 2FA user, but the systems ignores the second factor\. To enable 2FA for a CO user, see [Creating 2FA users](#create-2fa) or [Managing 2FA for HSM users](#rotate-2fa)\. 

1.  Use loginHSM to log in with 2FA\. Specify that the 2FA user is a CO and provide the user name and password as normal, but use the `-2fa` parameter and include a location in the file system for the system to write the `authdata` file\. This file will include a digest for each HSM in the cluster\.

   ```
   aws-cloudhsm>loginHSM CO example-user <password> -2fa /path/to/authdata
   ```

   CMU prompts you to use the private key to sign the digests in the `authdata` file and return the signatures\.

1. Use the private key to sign the digests in the `authdata` file, add the signatures to the JSON formatted `authdata` file and then provide CMU with the location of the `authdata` file\. For more information, see [Configuration reference](#reference-2fa)\.

### Managing 2FA for HSM users<a name="rotate-2fa"></a>

Use change password to change the password for a 2FA user, or to enable or disable 2FA, or to rotate the 2FA key\. Each time you enable 2FA, you must provide a public key for 2FA logins\.

Change password performs any of the following scenarios: 
+ Change the password for a 2FA user
+ Change the password for a non\-2FA user
+ Add 2FA to a non\-2FA user
+ Remove 2FA from a 2FA user
+ Rotate the key for a 2FA user

You can also combine tasks\. For example, you can remove 2FA from a user and change the password at the same time, or you might rotate the 2FA key and change the user password\.

**To change passwords or rotate keys for CO users with 2FA enabled**

1. Use CMU to log in to the HSM as a CO with 2FA enabled\.

1.  Use changePswd to change the password or rotate the key from CO users with 2FA enabled\. Use the `-2fa` parameter and include a location in the file system for the system to write the `authdata` file\. This file includes a digest for each HSM in the cluster\.

   ```
   aws-cloudhsm>changePswd CO example-user <new-password> -2fa /path/to/authdata
   ```

   CMU prompts you to use the private key to sign the digests in the `authdata` file and return the signatures with the public key\.

1. Use the private key to sign the digests in the `authdata` file, add the signatures and the public key to the JSON formatted `authdata` file and then provide CMU with the location of the `authdata` file\. For more information, see [Configuration reference](#reference-2fa)\.
**Note**  
The cluster uses the same key for quorum authentication and 2FA\. If you are using quorum authentication or plan to use quorum authentication, see [Quorum authentication and 2FA](#quorum-2fa)\.

**To disable 2FA for CO users with 2FA enabled**

1. Use CMU to log in to the HSM as a CO with 2FA enabled\.

1.  Use changePswd to remove 2FA from CO users with 2FA enabled\. 

   ```
   aws-cloudhsm>changePswd CO example-user <new-password>
   ```

   CMU prompts you to confirm the change password operation\.
**Note**  
If you remove the 2FA requirement or change the password for a 2FA user that is also a quorum authentication user, you will also remove the registration of the quorum user as an MofN user\. For more information about quorum users and 2FA, see [Quorum authentication and 2FA](#quorum-2fa)\.

1. Type **y**\.

   CMU confirms the change password operation\.

### Configuration reference<a name="reference-2fa"></a>

The following is an example of the 2FA properties in the `authdata` file for both the CMU\-generated request and your responses\. 

```
{
    "Version": "1.0",
    "PublicKey": "-----BEGIN PUBLIC KEY----- ... -----END PUBLIC KEY-----",
    "Data": [
        {
            "HsmId": "hsm-lgavqitns2a",
            "Digest": "k5O1p3f6foQRVQH7S8Rrjcau6h3TYqsSdr16A54+qG8=",
            "Signature": "Kkdl ... rkrvJ6Q=="
        },
        {
            "HsmId": "hsm-lgavqitns2a",
            "Digest": "IyBcx4I5Vyx1jztwvXinCBQd9lDx8oQe7iRrWjBAi1w=",
            "Signature": "K1hxy ... Q261Q=="
        }
    ]
}
```

**Data**  
Top\-level node\. Contains a subordinate node for each HSM in the cluster\. Appears in requests and responses for all 2FA commands\.

**Digest**  
This is what you must sign to provide the second factor of authentication\. CMU generated in requests for all 2FA commands\.

**HsmId**  
The ID of your HSM\. Appears in requests and responses for all 2FA commands\.

**PublicKey**  
The public key portion of the key pair you generated inserted as PEM\-formatted string\. You enter this in responses for createUser and changePswd\.

**Signature**  
The Base 64 encoded signed digest\. You enter this in responses for all 2FA commands\.

**Version**  
The version of the authentication data JSON formatted file\. Appears in requests and responses for all 2FA commands\.