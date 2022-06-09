# Using trusted keys to control key unwraps<a name="cloudhsm_using_trusted_keys_control_key_wrap"></a>

AWS CloudHSM supports trusted key wrapping to protect data keys from insider threats\. This topic describes how to use the PKCS \#11 library attributes to create trusted keys to secure data, but you could also use attributes from the Java Cryptographic Extension \(JCE\) provider in the same way\.

**Topics**
+ [Understanding trusted keys](#understand-trusted-key-wraps)
+ [How to use trusted keys to control unwraps](#trusted_key_process)
+ [Code sample](#generate-key-unwrap-template-example)

## Understanding trusted keys<a name="understand-trusted-key-wraps"></a>

You specify actions permitted on a key with attributes that you define when you create or unwrap a key\. A *trusted key* is a key that you identify as trusted with an attribute \(`CKA_TRUSTED`\)\. You can only make this designation as the cryptographic officer \(CO\)\. On the trusted key, you define another attribute \(`CKA_UNWRAP_TEMPLATE`\) that specifies a *set* of attributes that data keys unwrapped by the trusted key must contain for the unwrap operation to succeed\. In this way, you ensure that your unwrapped data keys contain attributes that allow only the use you intend for those keys\. Meanwhile, you use another attribute \(`CKA_WRAP_WITH_TRUSTED`\) to identify all of the data keys you want to wrap with trusted keys\. In this way, you restrict data keys so that applications can only use trusted keys to unwrap them\. Once you set this attribute on the data keys, the attribute becomes read only and you cannot change it\. With these attributes in place, applications can only unwrap your data keys with the keys you trust, and these unwraps must always result in data keys that have attributes you intend to limit the use of those data keys\.

### Trusted key attributes<a name="key_attribute_background"></a>

In the PKCS \#11 library, use the following attributes to specify trusted keys to control key unwraps:
+ `CKA_WRAP_WITH_TRUSTED`: Apply this attribute to an exportable data key to specify that you can only wrap this key with keys marked as `CKA_TRUSTED`\. Once you set `CKA_WRAP_WITH_TRUSTED` to true, the attribute becomes read\-only and you cannot change or remove the attribute\.
+ `CKA_TRUSTED`: Apply this attribute to the wrapping key \(in addition to `CKA_UNWRAP_TEMPLATE`\) to specify that a CO has done the necessary diligence and trusts this key\. Only a CO can set `CKA_TRUSTED`\. The *CU* owns the key, but only a CO can set `CKA_TRUSTED`\.
+ `CKA_UNWRAP_TEMPLATE`: Apply this attribute to the wrapping key \(in addition to `CKA_TRUSTED`\) to specify which attribute names and values the service must automatically apply to data keys that the service unwraps\. When an application submits a key for unwrapping, the application can also provide its own unwrap template\. If you specify an unwrap template and the application provides its own unwrap template, the HSM uses the union of both templates to apply attribute names and values to the key\. However, if a value in the `CKA_UNWRAP_TEMPLATE` for the wrapping key conflicts with an attribute provided by the application during the unwrap request, then the unwrap request fails\. 

For more information about PKCS \#11 library attributes, see [Supported attributes](pkcs11-attributes.md)\.

## How to use trusted keys to control unwraps<a name="trusted_key_process"></a>

**Step 1: Generate the trusted key**

To set up a trusted key, you establish a CU account and generates the wrapping keys with the appropriate `CKA_UNWRAP_TEMPLATE` specification\. Generally, you should include `CKA_WRAP_WITH_TRUSTED = TRUE` as part of this template\. If you want the unwrapped keys to be non\-exportable, set `CKA_EXPORTABLE = FALSE`\. To generate the key, you must use a PKCS \#11 library application\. You can't use advanced attributes with command\-line tools\.

**Step 2: Mark the key as trusted**

1. To mark the key as trusted, you must use a cryptographic officer \(CO\) account with CloudHSM Management Utility \(CMU\)\. 

   ```
   loginHSM CO <user-name> <password> 
   ```

1. Use setAttribute with OBJ\_ATTR\_TRUSTED \(value 134\) set to TRUE\. 

   ```
   setAttribute HH 134 1
   ```

For more information about setAttribute, see [setAttribute](cloudhsm_mgmt_util-setAttribute.md)

**Step 3: Share the trusted key with the application**

To share the trusted key with the application, you must log in with the CU account you created in step 1\. Use the [shareKey](cloudhsm_mgmt_util-shareKey.md) command to share the trusted key you created in step 2 with any CU accounts used by applications\. 

Then, the application CU account can use the trusted keys for wrapping and unwrapping data keys\. However, users cannot modify attributes for keys shared with them, and no one can use CU accounts to modify the `CKA_TRUSTED` attribute on the key\. 

**Step 4: Wrap all the data keys**

To wrap all the data keys, you use the CU account to import or generate all data keys and wrap them with the trusted wrapping key\. You can externally store the wrapped keys, as necessary\. Since you can only unwrap data keys with the original wrapping keys, you must apply the appropriate template at unwrap\. An application can unwrap keys on demand as needed, and delete the unwrapped key from the HSM once the key is no longer required\.

## Code sample<a name="generate-key-unwrap-template-example"></a>

This example uses an unwrap template to generate a key\. 

```
std::vector CK_ATTRIBUTE unwrapTemplate = {
    CK_ATTRIBUTE { CKA_KEY_TYPE, &aes, sizeof(aes) },
    CK_ATTRIBUTE { CKA_CLASS, &aesClass, sizeof(aesClass) },
    CK_ATTRIBUTE { CKA_TOKEN, &trueValue, sizeof(trueValue) },
    CK_ATTRIBUTE { CKA_EXTRACTABLE, &falseValue, sizeof(falseValue) }
};   
std::vector CK_ATTRIBUTE pubAttributes = {
    CK_ATTRIBUTE { CKA_KEY_TYPE, &rsa, sizeof(rsa) },
    CK_ATTRIBUTE { CKA_CLASS, &pubClass, sizeof(pubClass) },
    CK_ATTRIBUTE { CKA_TOKEN, &trueValue, sizeof(trueValue) },
    CK_ATTRIBUTE { CKA_MODULUS_BITS, &modulusBits, sizeof(modulusBits) },
    CK_ATTRIBUTE { CKA_PUBLIC_EXPONENT, publicExponent.data(), publicExponent.size() },
    CK_ATTRIBUTE { CKA_VERIFY, &trueValue, sizeof(trueValue) },
    CK_ATTRIBUTE { CKA_WRAP, &trueValue, sizeof(trueValue) },
    CK_ATTRIBUTE { CKA_ENCRYPT, &trueValue, sizeof(trueValue) }
};
std::vector CK_ATTRIBUTE priAttributes = {
    CK_ATTRIBUTE { CKA_KEY_TYPE, &rsa, sizeof(rsa) },
    CK_ATTRIBUTE { CKA_CLASS, &priClass, sizeof(priClass) },
    CK_ATTRIBUTE { CKA_PRIVATE, &trueValue, sizeof(trueValue) },
    CK_ATTRIBUTE { CKA_TOKEN, &trueValue, sizeof(trueValue) },
    CK_ATTRIBUTE { CKA_SIGN, &trueValue, sizeof(trueValue) },
    CK_ATTRIBUTE { CKA_UNWRAP, &trueValue, sizeof(trueValue) },
    CK_ATTRIBUTE { CKA_DECRYPT, &trueValue, sizeof(trueValue) },
    CK_ATTRIBUTE { CKA_UNWRAP_TEMPLATE, unwrapTemplate.data(), unwrapTemplate.size() * sizeof(CK_ATTRIBUTE) }
};
funcs->C_GenerateKeyPair(
    session,
    &rsaMechanism,
    pubAttributes.data(),
    pubAttributes.size(),
    priAttributes.data(),
    priAttributes.size(),
    &pubKey,
    &priKey
);
```