# AWS CloudHSM Client and Software Version History<a name="client-and-software-version-history"></a>

The following topics contain the version history for the AWS CloudHSM client and software libraries\. 


+ [AWS CloudHSM Client](#client-version-history)
+ [PKCS \#11 Library](#pkcs11-version-history)
+ [OpenSSL Library](#openssl-version-history)
+ [Java Library](#java-version-history)

## AWS CloudHSM Client<a name="client-version-history"></a>

### Current Version: 1\.0\.11<a name="client-current-version"></a>

You can download the current version of the AWS CloudHSM client from [https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-latest.x86_64.rpm](https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-latest.x86_64.rpm)\. For more information, see [Install and Configure the Client](install-and-configure-client.md)\.

Significant changes in this version include the following:

+ Improved load balancing\.

+ Improved performance\.

+ Improved handling of lost server connections\.

### Previous Versions<a name="client-previous-versions"></a>

**Version 1\.0\.10**

You can download version 1\.0\.10 from [https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-1.0-10.x86_64.rpm](https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-1.0-10.x86_64.rpm)\.

Significant changes in this version include the following:

+ Updated the key\_mgmt\_util command line tool to enable AES wrapped key import\.

+ Improved performance\.

+ Fixed various bugs\.

**Version 1\.0\.8**

You can download version 1\.0\.8 from [https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-1.0-8.x86_64.rpm](https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-1.0-8.x86_64.rpm)\.

Significant changes in this version include the following:

+ Improved setup experience\.

+ Added respawning to the client upstart service\.

+ Fixed various bugs\.

**Version 1\.0\.7**

Significant changes in this version include the following:

+ Added the pkpspeed performance testing tool\. For more information, see [Verify the Performance of the HSM](troubleshooting-verify-hsm-performance.md)\.

+ Fixed bugs to improve stability and performance\.

**Version 1\.0\.0**

This is the initial release\.

## PKCS \#11 Library<a name="pkcs11-version-history"></a>

### Current Version: 1\.0\.11<a name="pkcs11-current-version"></a>

You can download the current version of the AWS CloudHSM software library for PKCS \#11 from [https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-pkcs11-latest.x86_64.rpm](https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-pkcs11-latest.x86_64.rpm)\. For more information, see [Installing the PKCS \#11 Library](pkcs11-library-install.md)\.

Significant changes in this version include the following:

+ Added support for the CKM\_RSA\_PKCS\_PSS sign/verify mechanism\.

### Previous Versions<a name="pkcs11-previous-versions"></a>

**Version 1\.0\.10**

You can download version 1\.0\.10 from [https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-pkcs11-1.0-10.x86_64.rpm](https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-pkcs11-1.0-10.x86_64.rpm)\.

Updated the version number for consistency\.

**Version 1\.0\.8**

Significant changes in this version include the following:

+ Fixed bugs to address relative paths in the Redis setup\.

**Version 1\.0\.7**

Significant changes in this version include the following:

+ Added an accelerated version of the library that uses a Redis local cache to improve performance\.

+ Fixed bugs related to attribute handling\.

+ Added the ability to generate ECDSA keys\.

**Version 1\.0\.0**

This is the initial release\.

## OpenSSL Library<a name="openssl-version-history"></a>

### Current Version: 1\.0\.11<a name="openssl-current-version"></a>

You can download the current version of the AWS CloudHSM software library for OpenSSL from [https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-dyn-latest.x86_64.rpm](https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-dyn-latest.x86_64.rpm)\. For more information, see [Installing the OpenSSL Library](openssl-library-install.md)\.

Significant changes in this version include the following:

+ Updated the version number for consistency\.

### Previous Versions<a name="openssl-previous-versions"></a>

**Version 1\.0\.10**

You can download version 1\.0\.10 from [https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-dyn-1.0-10.x86_64.rpm](https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-dyn-1.0-10.x86_64.rpm)\.

Updated the version number for consistency\.

**Version 1\.0\.8**

Significant changes in this version include the following:

+ Improved performance\.

**Version 1\.0\.7**

Updated the version number for consistency\.

**Version 1\.0\.0**

This is the initial release\.

## Java Library<a name="java-version-history"></a>

### Current Version: 1\.0\.11<a name="java-current-version"></a>

You can download the current version of the AWS CloudHSM software library for Java from [https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-jce-latest.x86_64.rpm](https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-jce-latest.x86_64.rpm)\. For more information, see [Installing the Java Library](java-library-install.md)\.

Significant changes in this version include the following:

+ Improved the performance of several algorithms\.

+ Added Triple DES \(3DES\) key import feature\.

+ Various bug fixes\.

### Previous Versions<a name="java-previous-versions"></a>

**Version 1\.0\.10**

You can download version 1\.0\.10 from [https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-jce-1.0-10.x86_64.rpm](https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-jce-1.0-10.x86_64.rpm)\.

Significant changes in this version include the following:

+ Added support for additional algorithms\.

+ Improved performance\.

**Version 1\.0\.8**

Significant changes in this version include the following:

+ Updated the version number for consistency\.

**Version 1\.0\.7**

Significant changes in this version include the following:

+ Added support for additional algorithms\.

+ Signed the JAR files for compatibility with the Sun JCE provider\.

**Version 1\.0\.0**

This is the initial release\.