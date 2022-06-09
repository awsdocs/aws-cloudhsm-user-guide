# Supported Java attributes for Client SDK 5<a name="java-lib-attributes_5"></a>

This topic describes how to use a proprietary extension for the JCE provider to set key attributes\. Use this extension to set supported key attributes and their values during these operations:
+ Key generation
+ Key import

For examples of how to use key attributes, see [Code samples for the AWS CloudHSM software library for Java for Client SDK 5](java-samples_5.md)\.

**Topics**
+ [Understanding attributes](#java-understanding-attributes_5)
+ [Supported attributes](#java-attributes_5)
+ [Setting attributes for a key](#java-setting-attributes_5)

## Understanding attributes<a name="java-understanding-attributes_5"></a>

Use key attributes to specify what actions are permitted on key objects, including public, private or secret keys\. Define key attributes and values during key object creation operations\. 

The Java Cryptography Extension \(JCE\) does not specify how you should set values on key attributes, so most actions were permitted by default\. In contrast, the PKCS\# 11 standard defines a comprehensive set of attributes with more restrictive defaults\. Starting with the JCE provider 3\.1, AWS CloudHSM provides a proprietary extension that enables you to set more restrictive values for commonly used attributes\. 

## Supported attributes<a name="java-attributes_5"></a>

You can set values for the attributes listed in the following table\. As a best practice, only set values for attributes you wish to make restrictive\. If you don’t specify a value, AWS CloudHSM uses the default value specified in the table below\. An empty cell in the Default Value columns indicates that there is no specific default value assigned to the attribute\.


****  

| Attribute | Default Value | Notes |  | Symmetric Key | Public Key in Key Pair | Private Key in Key Pair |  | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| DECRYPT | TRUE |  | TRUE | True indicates you can use the key to decrypt any buffer\. You generally set this to FALSE for a key whose WRAP is set to true\.  | 
| ENCRYPT | TRUE | TRUE |  | True indicates you can use the key to encrypt any buffer\. | 
| EXTRACTABLE | TRUE |  | TRUE | True indicates you can export this key out of the HSM\. | 
| LABEL |   |  |  | A user\-defined string\. It allows you to conveniently identify keys on your HSM\.  | 
| PRIVATE | TRUE | TRUE | TRUE | True indicates that a user may not access the key until the user is authenticated\. For clarity, users cannot access any keys on AWS CloudHSM until they are authenticated, even if this attribute is set to FALSE\. | 
| SIGN | TRUE |  | TRUE | True indicates you can use the key to sign a message digest\. This is generally set to FALSE for public keys and for private keys that you have archived\. | 
| TOKEN | FALSE | FALSE | FALSE |  A permanent key which is replicated across all HSMs in the cluster and included in backups\. TOKEN = FALSE implies an ephemeral key which is automatically erased when the connection to the HSM is broken or logged out\.  | 
| UNWRAP | TRUE |  | TRUE | True indicates you can use the key to unwrap \(import\) another key\. | 
| VERIFY | TRUE | TRUE |  | True indicates you can use the key to verify a signature\. This is generally set to FALSE for private keys\. | 
| WRAP | TRUE | TRUE |  | True indicates you can use the key to wrap another key\. You will generally set this to FALSE for private keys\. | 
| WRAP\_WITH\_TRUSTED | FALSE |  | FALSE | True indicates a key can only be wrapped and unwrapped with keys that have the TRUSTED attribute set to true\. Once a key has WRAP\_WITH\_TRUSTED set to true, that attribute is read\-only and can’t be set to false\. To read about trust wrapping, see [Using trusted keys to control key unwraps](https://docs.aws.amazon.com/cloudhsm/latest/userguide/cloudhsm_using_trusted_keys_control_key_wrap.html)\. | 

**Note**  
You get broader support for attributes in the PKCS\#11 library\. For more information, see [Supported PKCS \#11 Attributes](pkcs11-attributes.md)\.

## Setting attributes for a key<a name="java-setting-attributes_5"></a>

`KeyAttributesMap` is a Java Map\-like object, which you can use to set attribute values for key objects\. The methods for `KeyAttributesMap` function similar to the methods used for Java map manipulation\. 

To set custom values on attributes, you have two options:
+ Use the methods listed in the following table
+ Use builder patterns demonstrated later in this document

Attribute map objects support the following methods to set attributes:


****  

| Operation | Return Value | `KeyAttributesMap` method | 
| --- | --- | --- | 
| Get the value of a key attribute for an existing key | Object \(containing the value\) or null |  **get**\(keyAttribute\)  | 
| Populate the value of one key attribute  | The previous value associated with key attribute, or null if there was no mapping for a key attribute |  **put**\(keyAttribute, value\)  | 
| Populate values for multiple key attributes | N/A |  **putAll**\(keyAttributesMap\)  | 
| Remove a key\-value pair from the attribute map |  The previous value associated with key attribute, or *null* if there was no mapping for a key attribute  |  **remove**\(keyAttribute\)  | 

**Note**  
Any attributes you do not explicitly specify are set to the defaults listed in the preceding table in [Supported attributes](#java-attributes_5)\. 

### Setting attributes for a key pair<a name="java-setting-attributes-key-pair"></a>

Use the Java class `KeyPairAttributesMap` to handle key attributes for a key pair\. `KeyPairAttributesMap` encapsulates two `KeyAttributesMap` objects; one for a public key and one for a private key\.

To set individual attributes for the public key and private key separately, you can use the `put()` method on corresponding `KeyAttributes` map object for that key\. Use the `getPublic()` method to retrieve the attribute map for the public key, and use `getPrivate()` to retrieve the attribute map for the private key\. Populate the value of multiple key attributes together for both public and private key pairs using the `putAll()` with a key pair attributes map as its argument\.