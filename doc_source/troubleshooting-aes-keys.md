# Custom IVs with non\-compliant length for AES key wrap<a name="troubleshooting-aes-keys"></a>

This troubleshooting topic helps you determine if your application generates irrecoverable wrapped keys\. If you are impacted by this issue, use this topic to address the problem\.

**Topics**
+ [Determine whether your code generates irrecoverable wrapped keys](#troubleshooting-problem1)
+ [Actions you must take if your code generates irrecoverable wrapped keys](#troubleshooting-problem2)

## Determine whether your code generates irrecoverable wrapped keys<a name="troubleshooting-problem1"></a>

You are impacted only if you meet *all* the conditions below:


****  

| Condition | How do I know? | 
| --- | --- | 
|  Your application uses PKCS \#11 library   |  The PKCS \#11 library is installed as the `libpkcs11.so` file in your `/opt/cloudhsm/lib` folder\. Applications written in the C language generally use the PKCS \#11 library directly, while application written in Java may be using the library indirectly via a Java abstraction layer\. If you're using Windows, you are NOT affected, as PKCS \#11 library is not presently available for Windows\.  | 
|  Your application specifically uses version 3\.0\.0 of the PKCS \#11 library   |  If you received an email from the AWS CloudHSM team, you are likely using version 3\.0\.0 of the PKCS \#11 library\.  To check the software version on your application instances, use this command:  <pre>rpm -qa | grep ^cloudhsm</pre>  | 
|  You wrap keys using AES key wrapping  |  AES key wrapping means you use an AES key to wrap out some other key\. The corresponding mechanism name is `CKM_AES_KEY_WRAP`\. It is used with the function `C_WrapKey`\. Other AES based wrapping mechanisms that use initialization vectors \(IVs\), such as `CKM_AES_GCM` and` CKM_CLOUDHSM_AES_GCM`, are not affected by this issue\. [Learn more about functions and mechanisms](pkcs11-mechanisms.md)\.   | 
|  You specify a custom IV when calling AES key wrapping, and the length of this IV is shorter than 8  |  AES key wrap is generally initialized using a `CK_MECHANISM` structure as follows:  `CK_MECHANISM mech = {CKM_AES_KEY_WRAP, IV_POINTER, IV_LENGTH};` This issue applies to you only if: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/troubleshooting-aes-keys.html)  | 

If you do not meet all the conditions above, you may stop reading now\. Your wrapped keys can be unwrapped properly, and this issue does not impact you\. Otherwise, see [Actions you must take if your code generates irrecoverable wrapped keys](#troubleshooting-problem2)\. 

## Actions you must take if your code generates irrecoverable wrapped keys<a name="troubleshooting-problem2"></a>

You should take the following three steps: 

1.  **Immediately upgrade your PKCS \#11 library to a newer version**
   + [Latest PKCS \#11 library for Amazon Linux, CentOS 6 and RHEL 6](client-upgrade.md)
   + [Latest PKCS \#11 library for Amazon Linux 2, CentOS 7 and RHEL 7](client-upgrade.md)
   + [Latest PKCS \#11 library for Ubuntu 16\.04 LTS](client-upgrade.md)

1. **Update your software to use a standards\-compliant IV **

   We strongly recommend you follow our sample code and simply specify a NULL IV, which causes the HSM to utilize the standards\-compliant default IV\. Alternatively, you may explicitly specify the IV as `0xA6A6A6A6A6A6A6A6` with a corresponding IV length of `8`\. We do not recommend using any other IV for AES key wrapping, and will explicitly disable custom IVs for AES key wrapping in a future version of the PKCS \#11 library\.

   Sample code for properly specifying the IV appears in [aes\_wrapping\.c](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/blob/master/src/wrapping/aes_wrapping.c#L72) on GitHub\.

1. **Identify and recover existing wrapped keys**

   You should identify any keys you wrapped using version 3\.0\.0 of the PKCS \#11 library, and then contact support for assistance \([https://aws\.amazon\.com/support](https://aws.amazon.com/support)\) in recovering these keys\.

**Important**  
This issue only impacts keys wrapped with version 3\.0\.0 of the PKCS \#11 library\. You can wrap keys using earlier versions \(2\.0\.4 and lower\-numbered packages\) or later versions \(3\.0\.1 and higher\-numbered packages\) of the PKCS \#11 library\. 