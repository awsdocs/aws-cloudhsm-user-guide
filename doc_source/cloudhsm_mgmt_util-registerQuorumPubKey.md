# registerQuorumPubKey<a name="cloudhsm_mgmt_util-registerQuorumPubKey"></a>

 The registerQuorumPubKey command in cloudhsm\_mgmt\_util associates hardware security module \(HSM\) users with asymmetric RSA\-2048 key pairs\. Once you associate HSM users with keys, those users can use the private key to approve quorum requests and the cluster can use the registered public key to verify the signature is from the user\. For more information about quorum authentication, see [Managing Quorum Authentication \(M of N Access Control\)](quorum-authentication.md)\.

**Tip**  
In the AWS CloudHSM documentation, quorum authentication is sometimes referred to as M of N \(MofN\), which means a minimum of *M* approvers out of a total number *N* approvers\.

## User type<a name="registerQuorumPubKey-userType"></a>

The following types of users can run this command\.
+ Crypto officers \(CO\)

## Syntax<a name="registerQuorumPubKey-syntax"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
registerQuorumPubKey <user-type> <user-name> <registration-token> <signed-registration-token> <public-key>
```

## Examples<a name="registerQuorumPubKey-examples"></a>

This example shows how to use registerQuorumPubKey to register crypto officers \(CO\) as approvers on quorum authentication requests\. To run this command, you must have an asymmetric RSA\-2048 key pair, a signed token, and an unsigned token\. For more information about these requirements, see [Arguments](#registerQuorumPubKey-params)\.

**Example : Register a HSM user for quorum authentication**  
This example registers a CO named `quorum_officer` as an approver for quorum authentication\.   

```
aws-cloudhsm> registerQuorumPubKey CO <quorum_officer> </path/to/quorum_officer.token> </path/to/quorum_officer.token.sig> </path/to/quorum_officer.pub>

*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. AWS does NOT synchronize these changes automatically with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
registerQuorumPubKey success on server 0(10.0.0.1)
```
The final command uses the [listUsers](cloudhsm_mgmt_util-listUsers.md) command to verify that `quorum_officer` is registerd as an MofN user\.   

```
aws-cloudhsm> listUsers
Users on server 0(10.0.0.1):
Number of users found:3

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                    NO               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              quorum_officer                          YES               0               NO
```

## Arguments<a name="registerQuorumPubKey-params"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
registerQuorumPubKey <user-type> <user-name> <registration-token> <signed-registration-token> <public-key>
```

**<user\-type>**  
Specifies the type of user\. This parameter is required\.   
For detailed information about the user types on a HSM, see [Understanding HSM users](manage-hsm-users.md#understanding-users)\.  
Valid values:  
+ **CO**: Crypto officers can manage users, but they cannot manage keys\. 
Required: Yes

**<user\-name>**  
Specifies a friendly name for the user\. The maximum length is 31 characters\. The only special character permitted is an underscore \( \_ \)\.  
You cannot change the name of a user after it is created\. In cloudhsm\_mgmt\_util commands, the user type and password are case\-sensitive, but the user name is not\.  
Required: Yes

**<registration\-token>**  
Specifies the path to a file that contains an unsigned registration token\. Can have any random data of max file size of 245 bytes\. For more information about creating an unsigned registration token, see [Create and Sign a Registration Token](quorum-authentication-crypto-officers-first-time-setup.md#mofn-registration-token)\.  
Required: Yes

**<signed\-registration\-token>**  
Specifies the path to a file that contains the SHA256\_PKCS mechanism signed hash of the registration\-token\. For more information, see [Create and Sign a Registration Token](quorum-authentication-crypto-officers-first-time-setup.md#mofn-registration-token)\.  
Required: Yes

**<public\-key>**  
Specifies the path to a file that contains the public key of an asymmetric RSA\-2048 key pair\. Use the private key to sign the registration token\. For more information, see [Create an RSA Key Pair](quorum-authentication-crypto-officers-first-time-setup.md#mofn-key-pair-create)\.  
Required: Yes  
The cluster uses the same key for quorum authentication and for two\-factor authentication \(2FA\)\. This means you can't rotate a quorum key for a user that has 2FA enabled using registerQuorumPubKey\. To rotate the key, you must use changePswd\. For more information about using quorum authentication and 2FA, see [Quorum Authentication and 2FA](manage-2fa.md#quorum-2fa)\.

## Related topics<a name="registerQuorumPubKey-seealso"></a>
+ [Create an RSA Key Pair](quorum-authentication-crypto-officers-first-time-setup.md#mofn-key-pair-create)
+ [Create and Sign a Registration Token](quorum-authentication-crypto-officers-first-time-setup.md#mofn-registration-token)
+ [Register the Public Key with the HSM](quorum-authentication-crypto-officers-first-time-setup.md#mofn-register-key)
+ [Managing Quorum Authentication \(M of N Access Control\)](quorum-authentication.md)
+ [Quorum Authentication and 2FA](manage-2fa.md#quorum-2fa)
+ [listUsers](cloudhsm_mgmt_util-listUsers.md)