# AES key wrapping in AWS CloudHSM<a name="manage-aes-key-wrapping"></a>

This topic describes the options for AES key wrapping in AWS CloudHSM\. AES key wrapping uses an AES key \(the wrapping key\) to wrap another key of any type \(the target key\)\. You use key wrapping to protect stored keys or transmit keys over insecure networks\.

**Topics**
+ [Supported algorithms](#supported-types)
+ [Using AES key wrap in AWS CloudHSM](#use-aes-key-wrap)

## Supported algorithms<a name="supported-types"></a>

AWS CloudHSM offers three options for AES key wrapping, each based on how the target key is padded before being wrapped\. Padding is done automatically, in accordance with the algorithm you use, when you call key wrap\. The following table lists the supported algorithms and associated details to help you choose an appropriate wrapping mechanism for your application\.


| AES Key Wrap Algorithm | Specification | Supported Target Key Types | Padding Scheme | AWS CloudHSM Client Availability  | 
| --- | --- | --- | --- | --- | 
| AES Key Wrap with Zero Padding  | [RFC 5649](https://tools.ietf.org/html/rfc5649) and [SP 800 \- 38F](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-38F.pdf) | All | Adds zeros after key bits, if necessary, to block align | SDK 3\.1 and later | 
| AES Key Wrap with No Padding | [RFC 3394](https://tools.ietf.org/html/rfc3394) and [SP 800 \- 38F](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-38F.pdf) | Block\-aligned keys such as AES and 3DES  | None | SDK 3\.1 and later | 
| AES Key Wrap with PKCS \#5 Padding | None1 | All |  At least 8 bytes are added as per PKCS \#5 padding scheme to block align   | All | 

To learn how to use the AES key wrap algorithms from the preceding table in your application, see [Using AES Key Wrap in AWS CloudHSM\.](#use-aes-key-wrap)

### Understanding initialization vectors in AES key wrap<a name="understand-padding-iv"></a>

Prior to wrapping, CloudHSM appends an initialization vector \(IV\) to the target key for data integrity\. Each key wrap algorithm has specific restrictions on what type of IV is allowed\. To set the IV in AWS CloudHSM, you have two options:
+ Implicit: set the IV to NULL and CloudHSM uses the default value for that algorithm for wrap and unwrap operations \(recommended\)
+ Explicit: set the IV by passing the default IV value to the key wrap function 

**Important**  
You must understand what IV you are using in your application\. To unwrap the key, you must provide the same IV that you used to wrap the key\. If you use an implicit IV to wrap, then use an implicit IV to unwrap\. With an implicit IV, CloudHSM will use the default value to unwrap\. 

The following table describes permitted values for IVs, which the wrapping algorithm specifies\. 


****  

| AES Key Wrap Algorithm | Implicit IV | Explicit IV | 
| --- | --- | --- | 
| AES Key Wrap with Zero Padding  | Required Default value: \(IV calculated internally based on specification\) | Not allowed | 
| AES Key Wrap with No Padding | Allowed \(recommended\) Default value: `0xA6A6A6A6A6A6A6A6` | Allowed Only this value accepted: `0xA6A6A6A6A6A6A6A6` | 
| AES Key Wrap with PKCS \#5 Padding | Allowed \(recommended\) Default value: `0xA6A6A6A6A6A6A6A6` | Allowed Only this value accepted: `0xA6A6A6A6A6A6A6A6` | 

## Using AES key wrap in AWS CloudHSM<a name="use-aes-key-wrap"></a>

 You wrap and unwrap keys as follows:
+ In the [PKCS \#11 library](pkcs11-library.md), select the appropriate mechanism for the `C_WrapKey` and `C_UnWrapKey` functions as shown in the following table\. 
+ In the [JCE provider](java-library.md), select the appropriate algorithm, mode and padding combination, implementing cipher methods `Cipher.WRAP_MODE` and `Cipher.UNWRAP_MODE` as shown in the following table\.
+ In [key\_mgmt\_util \(KMU\)](key_mgmt_util.md), use commands `wrapKey` and `unWrapKey` with appropriate m values as shown in the following table\. 


****  

| AES Key Wrap Algorithm | PKCS \#11 Mechanism | Java Method | KMU Argument | 
| --- | --- | --- | --- | 
| AES Key Wrap with Zero Padding  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/manage-aes-key-wrapping.html)  | AESWrap/ECB/ZeroPadding | m = 6 | 
| AES Key Wrap with No Padding |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/manage-aes-key-wrapping.html)  | AESWrap/ECB/NoPadding | m = 5 | 
| AES Key Wrap with PKCS \#5 Padding  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/manage-aes-key-wrapping.html)  | AESWrap/ECB/PKCS5Padding | m = 4 | 

1The `CKM_AES_KEY_WRAP` mechanism is not compliant with the PKCS \#11 2\.40 specification\. For more information, see the first known issue under [Known Issues for all HSM instances](ki-all.md)\.