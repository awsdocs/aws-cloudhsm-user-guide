# Key Attribute Reference<a name="key-attribute-table"></a>

The key\_mgmt\_util commands use constants to represent the attributes of keys in an HSM\. This topic can help you to identify the attributes, find the constants that represent them in commands, and understand their values\. 

You set the attributes of a key when you create it\. To change the token attribute, which indicates whether a key is persistent or exists only in the session, use the [setAttribute](key_mgmt_util-setAttribute.md) command in key\_mgmt\_util\. To change the label, wrap, unwrap, encrypt, or decrypt attributes, use the `setAttribute` command in cloudhsm\_mgmt\_util\.

To get a list of attributes and their constants, use [listAttributes](key_mgmt_util-listAttributes.md)\. To get the attribute values for a key, use [getAttribute](key_mgmt_util-getAttribute.md)\.

The following table lists the key attributes, their constants, and their valid values\.


| Attribute | Constant | Values | 
| --- | --- | --- | 
|  OBJ\_ATTR\_CLASS  |  0  | **2**: Public key in a public–private key pair\.3: Private key in a public–private key pair\.**4**: Secret \(symmetric\) key\. | 
|  OBJ\_ATTR\_TOKEN  |  1  |  **0**: False\. Session key\. **1**: True\. Persistent key\.  | 
|  OBJ\_ATTR\_PRIVATE  |  2  |  **0**: False\.  **1**: True\. This attribute indicates whether unauthenticated users can list the attributes of the key\. Since the CloudHSM PKCS\#11 provider currently does not support public sessions, all keys \(including public keys in a public\-private key pair\) have this attribute set to 1\.  | 
|  OBJ\_ATTR\_LABEL  |  3  | User\-defined string\. It does not have to be unique in the cluster\. | 
|  OBJ\_ATTR\_TRUSTED  |  134  |  **0**: False\. **1**: True\.  | 
|  OBJ\_ATTR\_KEY\_TYPE  | 256 |  **0**: RSA\.**1**: DSA\.**3**: EC\. **16**: Generic secret\. **18**: RC4\. **21**: Triple DES \(3DES\)\. **31**: AES\. | 
|  OBJ\_ATTR\_ID  | 258 |  User\-defined string\. Must be unique in the cluster\. The default is an empty string\. | 
|  OBJ\_ATTR\_SENSITIVE  |  259  |  **0**: False\. Public key in a public–private key pair\. **1**: True\.   | 
|  OBJ\_ATTR\_ENCRYPT  |  260  |  **0**: False\.  **1**: True\. The key can be used to encrypt data\.   | 
|  OBJ\_ATTR\_DECRYPT  |  261  |  **0**: False\.  **1**: True\. The key can be used to decrypt data\.  | 
|  OBJ\_ATTR\_WRAP  |  262  |  **0**: False\.  **1**: True\. The key can be used to encrypt keys\.  | 
|  OBJ\_ATTR\_UNWRAP  |  263  |  **0**: False\.  **1**: True\. The key can be used to decrypt keys\.  | 
|  OBJ\_ATTR\_SIGN  |  264  |  **0**: False\.  **1**: True\. The key can be used for signing \(private keys\)\.  | 
|  OBJ\_ATTR\_VERIFY  |  266  |  **0**: False\.  **1**: True\. The key can be used for verification \(public keys\)\.  | 
|  OBJ\_ATTR\_DERIVE  |  268  |  **0**: False\. **1**: True\. The function derives the key\.   | 
|  OBJ\_ATTR\_MODULUS  |  288  |  The modulus that was used to generate an RSA key pair\.  For other key types, this attribute does not exist\.  | 
|  OBJ\_ATTR\_MODULUS\_BITS  |  289  |  The length of the modulus used to generate an RSA key pair\. For other key types, this attribute does not exist\.  | 
|  OBJ\_ATTR\_PUBLIC\_EXPONENT  |  290  |  The public exponent used to generate an RSA key pair\. For other key types, this attribute does not exist\.  | 
|  OBJ\_ATTR\_VALUE\_LEN  |  353  |  Key length in bytes\.  | 
|  OBJ\_ATTR\_EXTRACTABLE  |  354  |  **0**: False\.  **1**: True\. The key can be exported from the HSMs\.  | 
|  OBJ\_ATTR\_LOCAL  |  355  |  **0**\. False\. The key was imported into the HSMs\. **1**: True\.   | 
|  OBJ\_ATTR\_NEVER\_EXTRACTABLE  |  356  |  **0**: False\.  **1**: True\. The key cannot be exported from the HSMs\.   | 
|  OBJ\_ATTR\_ALWAYS\_SENSITIVE  |  357  |  **0**: False\.  **1**: True\.   | 
|  OBJ\_ATTR\_DESTROYABLE  |  370  |  **0**: False\.  **1**: True\.   | 
|  OBJ\_ATTR\_KCV  |  371  |  Key check value of the key\. For more information, see [Additional Details](#key-attribute-table-details)\.  | 
|  OBJ\_ATTR\_ALL  |  512  |  Represents all attributes\.  | 
|  OBJ\_ATTR\_WRAP\_WITH\_TRUSTED  |  528  |  **0**: False\.  **1**: True\.   | 
|  OBJ\_ATTR\_EKCV  |  4099  |  EKCV is a check sum value generated using the key bytes\.   | 
|  OBJ\_ATTR\_WRAP\_TEMPLATE  |  1073742353  |  Values should use the attribute template to match the key wrapped using this wrapping key\.\.   | 
|  OBJ\_ATTR\_UNWRAP\_TEMPLATE  |  1073742354  |  Values should use the attribute template applied to any key unwrapped using this wrapping key\.   | 

## Additional Details<a name="key-attribute-table-details"></a>

**Key check value \(kcv\)**  
The *key check value* \(KCV\) is a 3\-byte hash or checksum of a key that is generated when the HSM imports or generates a key\. You can also calculate a KCV outside of the HSM, such as after you export a key\. You can then compare the KCV values to confirm the identity and integrity of the key\. To get the KCV of a key, use [getAttribute](key_mgmt_util-getAttribute.md)\.  
AWS CloudHSM uses the following standard method to generate a key check value:  
+ **Symmetric keys**: First 3 bytes of the result of encrypting a zero\-block with the key\.
+ **Asymmetric key pairs**: First 3 bytes of the SHA\-1 hash of the public key\.
+ **HMAC keys**: KVC for HMAC keys is not supported at this time\.