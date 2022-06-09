# Supported Java attributes for Client SDK 3<a name="java-lib-attributes"></a>

This topic describes how to use a proprietary extension for the Java library version 3\.1 to set key attributes\. Use this extension to set supported key attributes and their values during these operations:
+ Key generation
+ Key import
+ Key unwrap

**Note**  
The extension for setting custom key attributes is an optional feature\. If you already have code that functions in Java library version 3\.0, you do not need to modify that code\. Keys you create will continue to contain the same attributes as before\. 

**Topics**
+ [Understanding attributes](#java-understanding-attributes)
+ [Supported attributes](#java-attributes)
+ [Setting attributes for a key](#java-setting-attributes)
+ [Putting it all together](#java-attributes-summary)

## Understanding attributes<a name="java-understanding-attributes"></a>

You use key attributes to specify what actions are permitted on key objects, including public, private or secret keys\. You define key attributes and values during key object creation operations\. 

However, the Java Cryptography Extension \(JCE\) does not specify how you should set values on key attributes, so most actions were permitted by default\. In contrast, the PKCS\# 11 standard defines a comprehensive set of attributes with more restrictive defaults\. Starting with the Java library version 3\.1, CloudHSM provides a proprietary extension that enables you to set more restrictive values for commonly used attributes\. 

## Supported attributes<a name="java-attributes"></a>

You can set values for the attributes listed in the table below\. As a best practice, only set values for attributes you wish to make restrictive\. If you don’t specify a value, CloudHSM uses the default value specified in the table below\. An empty cell in the Default Value columns indicates that there is no specific default value assigned to the attribute\.


****  

| Attribute | Default Value | Notes | 
| --- | --- | --- | 
|  | Symmetric Key | Public Key in Key Pair | Private Key in Key Pair |  | 
| CKA\_TOKEN | FALSE | FALSE | FALSE | A permanent key which is replicated across all HSMs in the cluster and included in backups\. CKA\_TOKEN = FALSE implies a session key, which is only loaded onto one HSM and automatically erased when the connection to the HSM is broken\. | 
| CKA\_LABEL |   |  |  | A user\-defined string\. It allows you to conveniently identify keys on your HSM\.  | 
| CKA\_EXTRACTABLE | TRUE |  | TRUE | True indicates you can export this key out of the HSM\. | 
| CKA\_ENCRYPT | TRUE | TRUE |  | True indicates you can use the key to encrypt any buffer\. | 
| CKA\_DECRYPT | TRUE |  | TRUE | True indicates you can use the key to decrypt any buffer\. You generally set this to FALSE for a key whose CKA\_WRAP is set to true\.  | 
| CKA\_WRAP | TRUE | TRUE |  | True indicates you can use the key to wrap another key\. You will generally set this to FALSE for private keys\. | 
| CKA\_UNWRAP | TRUE |  | TRUE | True indicates you can use the key to unwrap \(import\) another key\. | 
| CKA\_SIGN | TRUE |  | TRUE | True indicates you can use the key to sign a message digest\. This is generally set to FALSE for public keys and for private keys that you have archived\. | 
| CKA\_VERIFY | TRUE | TRUE |  | True indicates you can use the key to verify a signature\. This is generally set to FALSE for private keys\. | 
| CKA\_PRIVATE | TRUE | TRUE | TRUE | True indicates that a user may not access the key until the user is authenticated\. For clarity, users cannot access any keys on CloudHSM until they are authenticated, even if this attribute is set to FALSE\. | 

**Note**  
You get broader support for attributes in the PKCS\#11 library\. For more information, see [Supported PKCS \#11 Attributes](pkcs11-attributes.md)\.

## Setting attributes for a key<a name="java-setting-attributes"></a>

`CloudHsmKeyAttributesMap` is a [Java Map](https://devdocs.io/openjdk~8/java/util/map)\-like object, which you can use to set attribute values for key objects\. The methods for `CloudHsmKeyAttributesMap` function similar to the methods used for Java map manipulation\. 

To set custom values on attributes, you have two options:
+ Use the methods listed in the following table
+ Use builder patterns demonstrated later in this document

Attribute map objects support the following methods to set attributes:


****  

| Operation | Return Value | `CloudHSMKeyAttributesMap` method | 
| --- | --- | --- | 
| Get the value of a key attribute for an existing key | Object \(containing the value\) or null |  **get**\(keyAttribute\)  | 
| Populate the value of one key attribute  | The previous value associated with key attribute, or null if there was no mapping for a key attribute |  **put**\(keyAttribute, value\)  | 
| Populate values for multiple key attributes | N/A |  **putAll**\(keyAttributesMap\)  | 
| Remove a key\-value pair from the attribute map |  The previous value associated with key attribute, or *null* if there was no mapping for a key attribute  |  **remove**\(keyAttribute\)  | 

**Note**  
Any attributes you do not explicitly specify are set to the defaults listed in the preceding table in [Supported attributes](#java-attributes)\. 

### Builder pattern example<a name="java-setting-attributes-builder-example"></a>

Developers will generally find it more convenient to utilize classes through the Builder pattern\. As examples:

```
import com.amazonaws.cloudhsm.CloudHsmKeyAttributes;
import com.amazonaws.cloudhsm.CloudHsmKeyAttributesMap;
import com.amazonaws.cloudhsm.CloudHsmKeyPairAttributesMap;

CloudHsmKeyAttributesMap keyAttributesSessionDecryptionKey = 
   new CloudHsmKeyAttributesMap.Builder()
      .put(CloudHsmKeyAttributes.CKA_LABEL, "ExtractableSessionKeyEncryptDecrypt")
      .put(CloudHsmKeyAttributes.CKA_WRAP, false)
      .put(CloudHsmKeyAttributes.CKA_UNWRAP, false)
      .put(CloudHsmKeyAttributes.CKA_SIGN, false)
      .put(CloudHsmKeyAttributes.CKA_VERIFY, false)
      .build();

CloudHsmKeyAttributesMap keyAttributesTokenWrappingKey = 
   new CloudHsmKeyAttributesMap.Builder()
      .put(CloudHsmKeyAttributes.CKA_LABEL, "TokenWrappingKey")
      .put(CloudHsmKeyAttributes.CKA_TOKEN, true)
      .put(CloudHsmKeyAttributes.CKA_ENCRYPT, false)
      .put(CloudHsmKeyAttributes.CKA_DECRYPT, false)
      .put(CloudHsmKeyAttributes.CKA_SIGN, false)
      .put(CloudHsmKeyAttributes.CKA_VERIFY, false)
      .build();
```

Developers may also utilize pre\-defined attribute sets as a convenient way to enforce best practices in key templates\. As an example:

```
//best practice template for wrapping keys

CloudHsmKeyAttributesMap commonKeyAttrs = new CloudHsmKeyAttributesMap.Builder()
    .put(CloudHsmKeyAttributes.CKA_EXTRACTABLE, false)
    .put(CloudHsmKeyAttributes.CKA_DECRYPT, false)
    .build();

// initialize a new instance of CloudHsmKeyAttributesMap by copying commonKeyAttrs
// but with an appropriate label

CloudHsmKeyAttributesMap firstKeyAttrs = new CloudHsmKeyAttributesMap(commonKeyAttrs);
firstKeyAttrs.put(CloudHsmKeyAttributes.CKA_LABEL, "key label");

// alternatively, putAll() will overwrite existing values to enforce conformance

CloudHsmKeyAttributesMap secondKeyAttrs = new CloudHsmKeyAttributesMap();
secondKeyAttrs.put(CloudHsmKeyAttributes.CKA_DECRYPT, true);
secondKeyAttrs.put(CloudHsmKeyAttributes.CKA_ENCRYPT, true);
secondKeyAttrs.put(CloudHsmKeyAttributes.CKA_LABEL, “safe wrapping key”);
secondKeyAttrs.putAll(commonKeyAttrs); // will overwrite CKA_DECRYPT to be FALSE
```

### Setting attributes for a key pair<a name="java-setting-attributes-key-pair"></a>

Use the Java class `CloudHsmKeyPairAttributesMap` to handle key attributes for a key pair\. `CloudHsmKeyPairAttributesMap` encapsulates two `CloudHsmKeyAttributesMap` objects; one for a public key and one for a private key\.

To set individual attributes for the public key and private key separately, you can use the `put()` method on corresponding `CloudHsmKeyAttributes` map object for that key\. Use the `getPublic()` method to retrieve the attribute map for the public key, and use `getPrivate()` to retrieve the attribute map for the private key\. Populate the value of multiple key attributes together for both public and private key pairs using the `putAll()` with a key pair attributes map as its argument\.

### Builder pattern example<a name="java-setting-attributes-key-pair-builder-example"></a>

Developers will generally find it more convenient to set key attributes through the Builder pattern\. For example:

```
import com.amazonaws.cloudhsm.CloudHsmKeyAttributes;
import com.amazonaws.cloudhsm.CloudHsmKeyAttributesMap;
import com.amazonaws.cloudhsm.CloudHsmKeyPairAttributesMap;

//specify attributes up-front 
CloudHsmKeyAttributesMap keyAttributes = 
    new CloudHsmKeyAttributesMap.Builder()
        .put(CloudHsmKeyAttributes.CKA_SIGN, false)
        .put(CloudHsmKeyAttributes.CKA_LABEL, "PublicCertSerial12345")
        .build();

CloudHsmKeyPairAttributesMap keyPairAttributes =
    new CloudHsmKeyPairAttributesMap.Builder()
        .withPublic(keyAttributes)
        .withPrivate(
            new CloudHsmKeyAttributesMap.Builder() //or specify them inline 
                .put(CloudHsmKeyAttributes.CKA_LABEL, "PrivateCertSerial12345")
                .put (CloudHSMKeyAttributes.CKA_WRAP, FALSE)
                .build()
        )
        .build();
```

**Note**  
For more information about this proprietary extension, see the [Javadoc](https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Docs/CloudHsm_CustomKeyAttributes_Javadoc.zip) archive and the [sample](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/CustomKeyAttributesRunner.java) on GitHub\. To explore the Javadoc, download and expand the archive\.

## Putting it all together<a name="java-attributes-summary"></a>

To specify key attributes with your key operations, follow these steps:

1. Instantiate `CloudHsmKeyAttributesMap` for symmetric keys or `CloudHsmKeyPairAttributesMap` for key pairs\.

1. Define the attributes object from step 1 with the required key attributes and values\.

1. Instantiate a `Cavium*ParameterSpec` class, corresponding to your specific key type, and pass into its constructor this configured attributes object\.

1. Pass this `Cavium*ParameterSpec` object into a corresponding crypto class or method\.

For reference, the following table contains the `Cavium*ParameterSpec` classes and methods which support custom key attributes\.


****  

| Key Type | Parameter Spec Class | Example Constructors | 
| --- | --- | --- | 
| Base Class | CaviumKeyGenAlgorithmParameterSpec | CaviumKeyGenAlgorithmParameterSpec\(CloudHsmKeyAttributesMap keyAttributesMap\) | 
| DES | CaviumDESKeyGenParameterSpec | CaviumDESKeyGenParameterSpec\(int keySize, byte\[\] iv, CloudHsmKeyAttributesMap keyAttributesMap\) | 
| RSA | CaviumRSAKeyGenParameterSpec | CaviumRSAKeyGenParameterSpec\(int keysize, BigInteger publicExponent, CloudHsmKeyPairAttributesMap keyPairAttributesMap\) | 
| Secret | CaviumGenericSecretKeyGenParameterSpec | CaviumGenericSecretKeyGenParameterSpec\(int size, CloudHsmKeyAttributesMap keyAttributesMap\) | 
| AES | CaviumAESKeyGenParameterSpec | CaviumAESKeyGenParameterSpec\(int keySize, byte\[\] iv, CloudHsmKeyAttributesMap keyAttributesMap\) | 
| EC | CaviumECGenParameterSpec | CaviumECGenParameterSpec\(String stdName, CloudHsmKeyPairAttributesMap keyPairAttributesMap\) | 

### Sample code: Generate and wrap a key<a name="example-generate-wrap-key"></a>

These brief code samples demonstrate the steps for two different operations: Key Generation and Key Wrapping:

```
// Set up the desired key attributes

KeyGenerator keyGen = KeyGenerator.getInstance("AES", "Cavium");
CaviumAESKeyGenParameterSpec keyAttributes = new CaviumAESKeyGenParameterSpec(
    256,
    new CloudHsmKeyAttributesMap.Builder()
        .put(CloudHsmKeyAttributes.CKA_LABEL, "MyPersistentAESKey")
        .put(CloudHsmKeyAttributes.CKA_EXTRACTABLE, true)
        .put(CloudHsmKeyAttributes.CKA_TOKEN, true)
        .build()
);

// Assume we already have a handle to the myWrappingKey
// Assume we already have the wrappedBytes to unwrap

// Unwrap a key using Custom Key Attributes

CaviumUnwrapParameterSpec unwrapSpec = new CaviumUnwrapParameterSpec(myInitializationVector, keyAttributes);

Cipher unwrapCipher = Cipher.getInstance("AESWrap", "Cavium");
unwrapCipher.init(Cipher.UNWRAP_MODE, myWrappingKey, unwrapSpec);
Key unwrappedKey = unwrapCipher.unwrap(wrappedBytes, "AES", Cipher.SECRET_KEY);
```