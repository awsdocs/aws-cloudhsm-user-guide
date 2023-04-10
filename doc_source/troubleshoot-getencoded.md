# Extracting keys using JCE<a name="troubleshoot-getencoded"></a>

## getEncoded, getPrivateExponent, or getS returns null<a name="w13aac30c13b3"></a>

`getEncoded`, `getPrivateExponent`, and `getS` will return null because they are by default disabled\. To enable them, refer to [Key extraction using JCE](java-lib-configs-getencoded.md)\.

If `getEncoded`, `getPrivateExponent`, and `getS` return null after being enabled, your key does not meet the right prerequisites\. For more information, refer to [Key extraction using JCE](java-lib-configs-getencoded.md)\.

## getEncoded, getPrivateExponent, or getS return key bytes outside of the HSM<a name="w13aac30c13b5"></a>

You or someone with access to your system has enabled clear key extraction\. See the following pages for more information, including how to reset this configuration to the default disabled state\.
+ [Key extraction using JCE](java-lib-configs-getencoded.md)
+ [Protecting and extracting keys from an HSM](best-practices.md#best-practices-key-protection)