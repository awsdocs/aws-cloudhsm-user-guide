# Supported attributes \(Client SDK 3\)<a name="pkcs11-v3-attributes"></a>

A key object can be a public, private, or secret key\. Actions permitted on a key object are specified through attributes\. Attributes are defined when the key object is created\. When you use the PKCS \#11 library, we assign default values as specified by the PKCS \#11 standard\.

AWS CloudHSM does not support all attributes listed in the PKCS \#11 specification\. We are compliant with the specification for all attributes we support\. These attributes are listed in the respective tables\.

Cryptographic functions such as `C_CreateObject`, `C_GenerateKey`, `C_GenerateKeyPair`, `C_UnwrapKey`, and `C_DeriveKey` that create, modify, or copy objects take an attribute template as one of their parameters\. For more information about passing an attribute template during object creation, see [Generate keys through PKCS \#11 library](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/tree/master/src/generate) sample\.

## Interpreting the PKCS \#11 library attributes table<a name="pkcs11-v3-attributes-interpreting"></a>

The PKCS \#11 library table contains a list of attributes that differ by key types\. It indicates whether a given attribute is supported for a particular key type when using a specific cryptographic function with AWS CloudHSM\.

**Legend:**
+ ✔ indicates that CloudHSM supports the attribute for the specific key type\.
+ ✖ indicates that CloudHSM does not support the attribute for the specific key type\.
+ R indicates that the attribute value is set to read\-only for the specific key type \.
+ S indicates that the attribute cannot be read by the `GetAttributeValue` as it is sensitive\.
+ An empty cell in the Default Value column indicates that there is no specific default value assigned to the attribute\.

## GenerateKeyPair<a name="pkcs11-v3-generatekeypair"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/pkcs11-v3-attributes.html)

## GenerateKey<a name="pkcs11-v3-generatekey"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/pkcs11-v3-attributes.html)

## CreateObject<a name="pkcs11-v3-createobject"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/pkcs11-v3-attributes.html)

## UnwrapKey<a name="pkcs11-v3-unwrapkey"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/pkcs11-v3-attributes.html)

## DeriveKey<a name="pkcs11-v3-derivekey"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/pkcs11-v3-attributes.html)

## GetAttributeValue<a name="pkcs11-v3-getattributevalue"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/pkcs11-v3-attributes.html)

**Attribute annotations**
+ \[1\] This attribute is partially supported by the firmware and must be explicitly set only to the default value\.
+ \[2\] Mandatory attribute\.
+ \[3\] **Client SDK 3 only**\. The `CKA_SIGN_RECOVER` attribute is derived from the `CKA_SIGN` attribute\. If being set, it can only be set to the same value that is set for `CKA_SIGN`\. If not set, it derives the default value of `CKA_SIGN`\. Since CloudHSM only supports RSA\-based recoverable signature mechanisms, this attribute is currently applicable to RSA public keys only\.
+ \[4\] **Client SDK 3 only**\. The `CKA_VERIFY_RECOVER` attribute is derived from the `CKA_VERIFY` attribute\. If being set, it can only be set to the same value that is set for `CKA_VERIFY`\. If not set, it derives the default value of `CKA_VERIFY`\. Since CloudHSM only supports RSA\-based recoverable signature mechanisms, this attribute is currently applicable to RSA public keys only\.

## Modifying attributes<a name="pkcs11-v3-modify-attr"></a>

Some attributes of an object can be modified after the object has been created, whereas some cannot\. To modify attributes, use the [setAttribute](cloudhsm_mgmt_util-setAttribute.md) command from cloudhsm\_mgmt\_util\. You can also derive a list of attributes and the constants that represent them by using the [listAttribute](cloudhsm_mgmt_util-listAttributes.md) command from cloudhsm\_mgmt\_util\.

The following list displays attributes that are allowed for modification after object creation:
+ `CKA_LABEL`
+ `CKA_TOKEN`
**Note**  
Modification is allowed only for changing a session key to a token key\. Use the [setAttribute](key_mgmt_util-setAttribute.md) command from key\_mgmt\_util to change the attribute value\.
+ `CKA_ENCRYPT`
+ `CKA_DECRYPT`
+ `CKA_SIGN`
+ `CKA_VERIFY`
+ `CKA_WRAP`
+ `CKA_UNWRAP`
+ `CKA_LABEL`
+ `CKA_SENSITIVE`
+ `CKA_DERIVE`
**Note**  
This attribute supports key derivation\. It must be `False` for all public keys and cannot be set to `True`\. For secret and EC private keys, it can be set to `True` or `False`\.
+ `CKA_TRUSTED`
**Note**  
This attribute can be set to `True` or `False` by Crypto Officer \(CO\) only\.
+ `CKA_WRAP_WITH_TRUSTED`
**Note**  
Apply this attribute to an exportable data key to specify that you can only wrap this key with keys marked as `CKA_TRUSTED`\. Once you set `CKA_WRAP_WITH_TRUSTED` to true, the attribute becomes read\-only and you cannot change or remove the attribute\.

## Interpreting error codes<a name="pkcs11-v3-attr-errors"></a>

Specifying in the template an attribute that is not supported by a specific key results in an error\. The following table contains error codes that are generated when you violate specifications:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/pkcs11-v3-attributes.html)