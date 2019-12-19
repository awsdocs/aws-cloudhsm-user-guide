# Using Trusted Keys to Control Key Unwraps<a name="cloudhsm_using_trusted_keys_control_key_wrap"></a>

It is possible that exportable keys \(a data key for example\) on the HSM could be wrapped with an arbitrary key \(for example a bad wrapping key\) and that could result in loss of the data key\. Though this action would have to be initiated by a hostile insider, if this is a concern, there are solutions that address this concern\.

The first solution is to block all key exports from the cluster\. This solution has limitations because restricting key exports negatively impact applications that need to export and import data keys\. However, a more flexible solution is to use a key unwrap template along with the trusted key and wrap\-with\-trusted attributes\. AWS CloudHSM 3\.0 and higher supports trusted keys and unwrap templates\. This article explains how to use a key unwrap template along with trusted key and wrap\-with\-trusted attributes\. 

## Background<a name="key_attribute_background"></a>

A key attribute is a property associated with the key, within the HSM, that specifies the permissions associated with the key\. If you want to \.
+ CKA\_WRAP\_WITH\_TRUSTED: When applied to an exportable data key, this attribute ensures the data key can only be wrapped with a key that has been marked as "CKA\_TRUSTED" by a cryptographic officer\. Once set to true, this becomes a read\-only attribute and cannot be unset by the cryptographic user\.
+ CKA\_TRUSTED: This is the only attribute that is specified by a cryptographic officer, rather than the cryptographic user who is the owner of a key\. CKA\_TRUSTED indicates that the cryptographic officer has done the necessary diligence, and recognizes this key is trusted\.
+ CKA\_UNWRAP\_TEMPLATE: An attribute template is a collection of attribute names and values\. When an unwrap template is specified for a wrapping key, all attributes in that template are automatically applied to the unwrapped data key\. When an application submits a key for unwrapping, it can separately provide an unwrap template\. In this case, the HSM uses the union of both templates\. However, if a value in the CKA\_UNWRAP\_TEMPLATE for the wrapping key conflicts with an attribute provided by the application during the unwrap request, the unwrap request fails\. 

For more information about the PKCS\#11 attributes supported by AWS CloudHSM, see the article on [Supported PKCS \#11 Attributes](pkcs11-library.html#pkcs11-attributes)\.

## Process<a name="trusted_key_process"></a>

**Step 1: Generate the key bits for the trusted key**

To set up a trusted key, a security officer human establishes a cryptographic user \(CU\) account and generates the wrapping keys with the appropriate CKA\_UNWRAP\_TEMPLATE specification\. Generally, you should include CKA\_WRAP\_WITH\_TRUSTED = TRUE as part of this template\. If you want the unwrapped keys to be non\-exportable, set CKA\_EXPORTABLE = FALSE\. To generate the key, you must use a PKCS\#11 application\. The advanced attributes are not supported by command line tools\.

**Step 2: Mark the key as trusted**

The security officer human logs in to cloudhsm\_mgmt\_util with a cryptographic officer \(CO\) account\. To mark the key as trusted, call `setAttribute` with OBJ\_ATTR\_TRUSTED \(value 134\) set to TRUE\. To learn more about the `setAttribute` function, see the article on [setAttribute](cloudhsm_mgmt_util-setAttribute.html) 

```
      loginHSM CO <user-name> <password> 
      setAttribute HH 134 1
      quit
```

**Step 3: Share the trusted key with the application**

The security officer human logs in with the CU account and uses the [shareKey function](cloudhsm_mgmt_util-shareKey.html) to share the trusted wrapping keys with the CU accounts that will be used by the applications\. Then, the application CU account can use the trusted keys for wrapping and unwrapping data keys\. However, users cannot modify attributes for keys shared with them, and cryptographic user accounts cannot be used to modify the key's CKA\_TRUSTED attribute\. Once this is completed, the security officer can be assured the trusted wrapping keys will remain correct\. 

**Step 4: Generate and wrap out all the data keys**

Using their CU account, the security officer imports or generates all data keys, and wraps them with the trusted wrapping key\. The wrapped keys are stored externally, as necessary\. Since data keys can only be unwrapped with the original wrapping keys, the appropriate template must be applied at unwrap\. An application can unwrap keys on demand as needed, and delete the unwrapped key from the HSM once the key is no longer required\.

## Sample: Generate a Key with an Unwrap Template in PKCS \#11<a name="generate-key-unwrap-template-example"></a>

This example uses an unWrap template to generate a key\. 

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
           BOOST_TEST_CONTEXT("Generate RSA Key Pair with Unwrap Template") {
                   BOOST_TEST_REQUIRE(CKR_OK == PKCS11_INVOKE_NOEXCEPT(
                           get_module(),
                           C_GenerateKeyPair,
                           get_session().get_handle(),
                           &rsaMechanism,
                           pubAttributes.data(),
                           pubAttributes.size(),
                           priAttributes.data(),
                           priAttributes.size(),
                           &pubKey,
                           &priKey
                    ));
```