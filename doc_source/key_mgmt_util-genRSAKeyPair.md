# genRSAKeyPair<a name="key_mgmt_util-genRSAKeyPair"></a>

The genRSAKeyPair command in the key\_mgmt\_util tool generates an [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) asymmetric key pair\. You specify the key type, modulus length, and a public exponent\. The command generates a modulus of the specified length and creates the key pair\. You can assign an ID, share the key with other HSM users, create nonextractable keys and keys that expire when the session ends\. When the command succeeds, it returns a key handle that the HSM assigns to the key\. You can use the key handle to identify the key to other commands\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [log in](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

**Tip**  
To find the attributes of a key that you have created, such as the type, length, label, and ID, use [getAttribute](key_mgmt_util-getAttribute.md)\. To find the keys for a particular user, use [getKeyInfo](key_mgmt_util-getKeyInfo.md)\. To find keys based on their attribute values, use [findKey](key_mgmt_util-findKey.md)\. 

## Syntax<a name="genRSAKeyPair-syntax"></a>

```
genRSAKeyPair -h

genRSAKeyPair -m <modulus length>
              -e <public exponent> 
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

## Examples<a name="genRSAKeyPair-examples"></a>

These examples show how to use genRSAKeyPair to create asymmetric key pairs in your HSMs\.

**Example : Create and examine an RSA key pair**  
This command creates an RSA key pair with a 2048\-bit modulus and an exponent of 65537\. The output shows that the public key handle is `2100177` and the private key handle is `2100426`\.  

```
Command: genRSAKeyPair -m 2048 -e 65537 -l rsa_test 

Cfm3GenerateKeyPair returned: 0x00 : HSM Return: SUCCESS

        Cfm3GenerateKeyPair:    public key handle: 2100177    private key handle: 2100426

        Cluster Status:
        Node id 0 status: 0x00000000 : HSM Return: SUCCESS
        Node id 1 status: 0x00000000 : HSM Return: SUCCESS
```
The next command uses [getAttribute](key_mgmt_util-getAttribute.md) to get the attributes of the public key that we just created\. It writes the output to the `attr_2100177` file\. It is followed by a cat command that gets the content of the attribute file\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.  
The resulting hexadecimal values confirm that it is a public key \(`OBJ_ATTR_CLASS 0x02`\) with a type of RSA \(`OBJ_ATTR_KEY_TYPE 0x00`\)\. You can use this public key to encrypt \(`OBJ_ATTR_ENCRYPT 0x01`\), but not to decrypt \(`OBJ_ATTR_DECRYPT 0x00`\)\. The results also include the key length \(512, `0x200`\), the modulus, the modulus length \(2048, `0x800`\), and the public exponent \(65537, `0x10001`\)\.  

```
Command:  getAttribute -o 2100177 -a 512 -out attr_2100177

Attribute size: 801, count: 26
Written to: attr_2100177 file

        Cfm3GetAttribute returned: 0x00 : HSM Return: SUCCESS

$  cat attr_2100177
OBJ_ATTR_CLASS
0x02
OBJ_ATTR_KEY_TYPE
0x00
OBJ_ATTR_TOKEN
0x01
OBJ_ATTR_PRIVATE
0x01
OBJ_ATTR_ENCRYPT
0x01
OBJ_ATTR_DECRYPT
0x00
OBJ_ATTR_WRAP
0x01
OBJ_ATTR_UNWRAP
0x00
OBJ_ATTR_SIGN
0x00
OBJ_ATTR_VERIFY
0x01
OBJ_ATTR_LOCAL
0x01
OBJ_ATTR_SENSITIVE
0x00
OBJ_ATTR_EXTRACTABLE
0x01
OBJ_ATTR_LABEL
rsa_test
OBJ_ATTR_ID

OBJ_ATTR_VALUE_LEN
0x00000200
OBJ_ATTR_KCV
0xc51c18
OBJ_ATTR_MODULUS
0xbb9301cc362c1d9724eb93da8adab0364296bde7124a241087d9436b9be57e4f7780040df03c2c
1c0fe6e3b61aa83c205280119452868f66541bbbffacbbe787b8284fc81deaeef2b8ec0ba25a077d
6983c77a1de7b17cbe8e15b203868704c6452c2810344a7f2736012424cf0703cf15a37183a1d2d0
97240829f8f90b063dd3a41171402b162578d581980976653935431da0c1260bfe756d85dca63857
d9f27a541676cb9c7def0ef6a2a89c9b9304bcac16fdf8183c0a555421f9ad5dfeb534cf26b65873
970cdf1a07484f1c128b53e10209cc6f7ac308669112968c81a5de408e7f644fe58b1a9ae1286fec
b3e4203294a96fae06f8f0db7982cb5d7f
OBJ_ATTR_MODULUS_BITS
0x00000800
OBJ_ATTR_PUBLIC_EXPONENT
0x010001
OBJ_ATTR_TRUSTED
0x00
OBJ_ATTR_WRAP_WITH_TRUSTED
0x00
OBJ_ATTR_DESTROYABLE
0x01
OBJ_ATTR_DERIVE
0x00
OBJ_ATTR_ALWAYS_SENSITIVE
0x00
OBJ_ATTR_NEVER_EXTRACTABLE
0x00
```

**Example : Generate a shared RSA key pair**  
This command generates an RSA key pair and shares the private key with user 4, another CU on the HSM\. The command uses the `m_value` parameter to require at least two approvals before the private key in the pair can be used in a cryptographic operation\. When you use the `m_value` parameter, you must also use `-u` in the command and the `m_value` cannot exceed the total number of users \(number of values in `-u` \+ owner\)\.  

```
 Command:  genRSAKeyPair -m 2048 -e 65537 -l rsa_mofn -id rsa_mv2 -u 4 -m_value 2

        Cfm3GenerateKeyPair returned: 0x00 : HSM Return: SUCCESS

        Cfm3GenerateKeyPair:    public key handle: 27    private key handle: 28

        Cluster Error Status
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
```

## Parameters<a name="genRSAKeyPair-params"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-m**  
Specifies the length of the modulus in bits\. The minimum value is 2048\.   
Required: Yes

**\-e**  
Specifies the public exponent\. The value must be an odd number greater than or equal to 65537\.  
Required: Yes

**\-l**  
Specifies a user\-defined label for the key pair\. Type a string\. The same label applies to both keys in the pair  
You can use any phrase that helps you to identify the key\. Because the label does not have to be unique, you can use it to group and categorize keys\.   
Required: Yes

**\-id**  
Specifies a user\-defined identifier for the key pair\. Type a string that is unique in the cluster\. The default is an empty string\. The ID that you specify applies to both keys in the pair\.  
Default: No ID value\.  
Required: No

**\-min\_srv**  
Specifies the minimum number of HSMs on which the key is synchronized before the value of the `-timeout` parameter expires\. If the key is not synchronized to the specified number of servers in the time allotted, it is not created\.  
AWS CloudHSM automatically synchronizes every key to every HSM in the cluster\. To speed up your process, set the value of `min_srv` to less than the number of HSMs in the cluster and set a low timeout value\. Note, however, that some requests might not generate a key\.  
Default: 1  
Required: No

**\-m\_value**  
Specifies the number of users who must approve any cryptographic operation that uses the private key in the pair\. Type a value from `0` to `8`\.  
This parameter establishes a quorum authentication requirement for the private key\. The default value, `0`, disables the quorum authentication feature for the key\. When quorum authentication is enabled, the specified number of users must sign a token to approve cryptographic operations that use the private key, and operations that share or unshare the private key\.  
To find the `m_value` of a key, use [getKeyInfo](key_mgmt_util-getKeyInfo.md)\.  
This parameter is valid only when the `-u` parameter in the command shares the key pair with enough users to satisfy the `m_value` requirement\.  
Default: 0  
Required: No

**\-nex**  
Makes the private key nonextractable\. The private key that is generated cannot be [exported from the HSM](using-kmu.md#export-keys)\. Public keys are always extractable\.  
Default: Both the public and private keys in the key pair are extractable\.  
Required: No

**\-sess**  
Creates a key that exists only in the current session\. The key cannot be recovered after the session ends\.  
Use this parameter when you need a key only briefly, such as a wrapping key that encrypts, and then quickly decrypts, another key\. Do not use a session key to encrypt data that you might need to decrypt after the session ends\.  
To change a session key to a persistent \(token\) key, use [setAttribute](key_mgmt_util-setAttribute.md)\.  
Default: The key is persistent\.   
Required: No

**\-timeout**  
Specifies how long \(in seconds\) the command waits for a key to be synchronized to the number of HSMs specified by the `min_srv` parameter\.   
This parameter is valid only when the `min_srv` parameter is also used in the command\.  
Default: No timeout\. The command waits indefinitely and returns only when the key is synchronized to the minimum number of servers\.  
Required: No

**\-u**  
Shares the private key in the pair with the specified users\. This parameter gives other HSM crypto users \(CUs\) permission to use the private key in cryptographic operations\. Public keys can be used by any user without sharing\.  
Type a comma\-separated list of HSM user IDs, such as \-`u 5,6`\. Do not include the HSM user ID of the current user\. To find HSM user IDs of CUs on the HSM, use [listUsers](key_mgmt_util-listUsers.md)\. To share and unshare existing keys, use [shareKey](cloudhsm_mgmt_util-shareKey.md) in the cloudhsm\_mgmt\_util\.   
Default: Only the current user can use the private key\.   
Required: No

**\-attest**  
Runs an integrity check that verifies that the firmware on which the cluster runs has not been tampered with\.  
Default: No attestation check\.  
Required: No

## Related topics<a name="genRSAKeyPair-seealso"></a>
+ [genSymKey](key_mgmt_util-genSymKey.md)
+ [genDSAKeyPair](key_mgmt_util-genDSAKeyPair.md)
+ [genECCKeyPair](key_mgmt_util-genECCKeyPair.md)