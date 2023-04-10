# Build an application<a name="create-apps"></a>

Build applications and work with keys using AWS CloudHSM\. 

To get started creating and using keys in your new cluster, you must first create a hardware security module \(HSM\) user with CloudHSM Management Utility \(CMU\)\. For more information, see [Understanding HSM User Management Tasks](cli-users.md#understand-users), [Getting started with AWS CloudHSM Command Line Interface \(CLI\)](cloudhsm_cli-getting-started.md), and [How to Manage HSM Users](manage-hsm-users.md)\.

**Note**  
If using Client SDK 3, use [CloudHSM Management Utility \(CMU\)](cloudhsm_mgmt_util.md) instead of CloudHSM CLI\.

With HSM users in place, you can log in to the HSM and create and use keys with any of the following options: 
+ Use [key management utility, a command line tool](key_mgmt_util-getting-started.md)
+ Build a C application using the [PKCS \#11 library](pkcs11-library.md)
+ Build a Java application using the [JCE provider](java-library.md)
+ Use the [OpenSSL Dynamic Engine directly from the command line](openssl-library.md)
+ Use the OpenSSL Dynamic Engine for TLS offload with [NGINX and Apache web servers](ssl-offload.md)
+ Use the CNG and KSP providers to use AWS CloudHSM with [Microsoft Windows Server Certificate Authority \(CA\)](win-ca-overview.md)
+ Use the CNG and KSP providers to use AWS CloudHSM with [Microsoft Sign Tool](signtool.md)
+ Use the CNG and KSP providers for TLS offload with [Internet Information Server \(IIS\) web server](ssl-offload.md)