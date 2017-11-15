# genDSAKeyPair<a name="key_mgmt_util-genDSAKeyPair"></a>

The `genDSAKeyPair` command in the key\_mgmt\_util tool generates a [Digital Signing Algorithm](https://en.wikipedia.org/wiki/Digital_Signature_Algorithm) \(DSA\) key pair in your HSMs\. You must specify the modulus length; the command generates the modulus value\. You can also assign an ID, share the key with other HSM users, create non\-extractable keys, and create keys that expire when the session ends\. When the command succeeds, it returns the *key handles* that the HSM assigns to the public and private keys\. You can use the key handles to identify the keys to other commands\.

Before you run any key\_mgmt\_util command, you must start key\_mgmt\_util and login to the HSM as a crypto user \(CU\)\. 

**Tip**  
To find the attributes of a key that you have created, such as the type, size, label, and ID, use getAttribute\. To find the keys for a particular user, use getKeyInfo\. To find keys based on their attribute values, use findKey\. 

## Syntax<a name="genDSAKeyPair-syntax"></a>

```
genDSAKeyPair -h

genDSAKeyPair -m <modulus length> 
              -l <label> 
              [-id <key ID>] 
              [-min_srv <minimum number of servers>] 
              [-m_value <0..8>]
              [-nex] 
              [-sess] 
              [-timeout <number of seconds> ]
              [-u <user-ids>] 
              [-attest]
```

## Examples<a name="genDSAKeyPair-examples"></a>

**Example : Create a DSA Key Pair**  
This command creates a DSA key pair with a "DSA" label\. The output shows that the key handle of the public key is 19 and the handle of the private key is 21\.  

```
Command: genDSAKeyPair -m 2048 -l DSA

        Cfm3GenerateKeyPair: returned: 0x00 : HSM Return: SUCCESS

        Cfm3GenerateKeyPair:    public key handle: 19    private key handle: 21

        Cluster Error Status
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```

**Example : Create a Session\-Only DSA Key Pair**  
This command creates a DSA key pair that is valid only in the current session\. The command assigns a unique ID of "DSA\_temp\_pair" in addition to the required \(non\-unique\) label\. You might want to create a key pair like this to sign and verify a session\-only token\. The output shows that the key handle of the public key is 12 and the handle of the private key is 14\.  

```
Command: genDSAKeyPair -m 2048 -l DSA-temp -id DSA_temp_pair -sess

        Cfm3GenerateKeyPair: returned: 0x00 : HSM Return: SUCCESS

        Cfm3GenerateKeyPair:    public key handle: 12    private key handle: 14

        Cluster Error Status
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```
To confirm that the key pair exists only in the session, use the `-sess` parameter of findKey with a value of 1 \(True\)\.  

```
  Command: findKey -sess 1

  Total number of keys present 2

 number of keys matched from start index 0::1
12, 14

        Cluster Error Status
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS

        Cfm3FindKey returned: 0x00 : HSM Return: SUCCESS
```

**Example : Create a Shared, Non\-Extractable DSA Key Pair**  
This command creates a DSA key pair\. The private key is shared with three other users and it cannot be exported from the HSM\. Public keys can be used by any user and can always be extracted\.   

```
        Command:  genDSAKeyPair -m 2048 -l DSA -id DSA_shared_pair -nex -u 3,5,6

        Cfm3GenerateKeyPair: returned: 0x00 : HSM Return: SUCCESS

        Cfm3GenerateKeyPair:    public key handle: 11    private key handle: 19

        Cluster Error Status
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```

**Example : Create a quorum\-controlled key pair**  
This command creates a DSA key pair with the label `DSA-mV2`\. The command uses the `-u` parameter to share the private key with user 4 and 6\. It uses the `-m_value` parameter to require a quorum of at least 2 approvals for any cryptographic operations that use the private key\. The command also uses the `-attest` parameter to verify the integrity of the firmware on which the key pair is generated\.  
The output shows that the command generate a public key with key handle 12 and a private key with key handle 17, and that the attestation check on the cluster firmware passed\.  

```
        Command:  genDSAKeyPair -m 2048 -l DSA-mV2 -m_value 2 -u 4,6 -attest

        Cfm3GenerateKeyPair: returned: 0x00 : HSM Return: SUCCESS

        Cfm3GenerateKeyPair:    public key handle: 12    private key handle: 17

        Attestation Check : [PASS]

        Cluster Error Status
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```
This command uses use getKeyInfo on the private key \(key handle 17\)\. The output confirms that the key is owned by the current user \(user 3\) and that it is shared with users 4 and 6 \(and no others\)\. The output also shows that quorum authentication is enabled and the quorum size is 2\.  

```
        Command:  getKeyInfo -k 17

        Cfm3GetKey returned: 0x00 : HSM Return: SUCCESS

        Owned by user 3

        also, shared to following 2 user(s):

                 4
                 6
         2 Users need to approve to use/manage this key
```

## Parameters<a name="genDSAKeyPair-params"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-m**  
Specifies the length of the modulus in bits\. The only valid value is 2048\.  
Required: Yes

**\-l**  
Specifies a user\-defined label for the key\. Enter a string\.  
You can use any phrase that helps you to identify the key\. Because the label does not have to be unique, you can use it to group and categorize keys\.   
Required: Yes

**\-id**  
Specifies a user\-defined identifier for the key\. Enter a string that is unique in the cluster\. The default is an empty string\.  
Default: No ID value\.  
Required: No

**\-min\_srv**  
Specifies the minimum number of HSMs on which the key is synchronized before the value of the `-timeout` parameter expires\. If the key is not synchronized to the specified number of servers in the time allotted, it is not created\.  
AWS CloudHSM automatically synchronizes every key to every HSM in the cluster\. By setting the value of `min_srv` to less than the number of HSMs in the cluster and setting a low `timeout` value, you can speed up your process, although some requests might not generate a key\.  
Default: 1  
Required: No

**\-m\_value**  
Specifies the number of users who must approve any cryptographic operation that use the private key in the pair\. Enter a value from 0 to 8\.  
This parameter establishes a quorum authentication requirement for the private key\. The default value, 0, disables the quorum authentication feature for the key\. When quorum authentication is enabled, the specified number of users must sign a token to approve cryptographic operations that use the private key, and operations that share or unshare the private key\.  
To find the m\_value of a key, use getKeyInfo\.  
This parameter is valid only when the `-u` parameter in the command shares the key pair with enough users to satisfy the `m_value` requirement\.  
Default: 0  
Required: No

**\-nex**  
Makes the private key non\-extractable\. The private key that is generated cannot be exported from the HSM\. Public keys are always extractable\.  
Default: Both the public and private keys in the key pair are extractable\.  
Required: No

**\-sess**  
Creates a key that exists only in the current session\. The key cannot be recovered after the session ends\.  
Use this parameter when you need a key only briefly, such as a wrapping key that encrypts, then quickly decrypts, another key\. Do not use a session key to encrypt data that you might need to decrypt after the session ends\.  
To change a session key to a persistent \("token"\) key, use setAttribute\.  
Default: The key is persistent\.   
Required: No

**\-timeout**  
Specifies how long \(in seconds\) the command waits for a key to be synchronized to the number of HSMs specified by the `min_srv` parameter\.   
This parameter is valid only when the `min_srv` parameter is also used in the command\.  
Default: No timeout\. The command waits indefinitely and returns only when the key is synchronized to the minimum number of servers\.  
Required: No

**\-u**  
Shares the private key in the pair with the specified users\. This parameter gives other HSM crypto users \(CUs\) permission to use the private key in cryptographic operations\. Public keys can be used by any user without sharing\.  
Enter a comma\-separated list of HSM user IDs, such as \-`u 5,6`\. Do not include the HSM user ID of the current user\. To find HSM user IDs of CUs on the HSM, use listUsers\. To share and unshare existing keys, use shareKey\.   
Default: Only the current user can use the private key\.   
Required: No

**\-attest**  
Runs an integrity check that verifies that the firmware on which the cluster runs has not been tampered\.  
Default: No attestation check\.  
Required: No

## Related Topics<a name="genDSAKeyPair-seealso"></a>

+ genRSAKeyPair

+ genSymKey

+ genECCKeyPair 