# Client SDK 3 supported platforms<a name="sdk3-support"></a>

Client SDK 3 requires a client daemon and offers command\-line tools including, CloudHSM Management Utility \(CMU\), key management utility \(KMU\), and the configure tool\. 

Base support is different for each version of the AWS CloudHSM Client SDK\. Typically platform support for components in an SDK matches base support, but not always\. To determine platform support for a given component, first make sure the platform you want appears in the base section for the SDK, then check for an exclusions or any other pertinent information in the component section\.

Platform support changes over time\. Earlier versions of the CloudHSM Client SDK may not support all the operating systems listed here\. Use release notes to determine the operating system support for previous versions of the CloudHSM Client SDK\. For more information, see [Downloads for AWS CloudHSM Client SDK](client-history.md)\.

AWS CloudHSM supports only 64\-bit operating systems\.

**Contents**
+ [Linux support](#sdk3-linux)
+ [Windows support](#sdk3-windows)
+ [Components support](#sdk3-support-components)
  + [PKCS \#11 library](#sdk3-support-pkcs11)
  + [JCE provider](#sdk3-support-jce)
  + [OpenSSL Dynamic Engine](#sdk3-support-openssl)
  + [CNG and KSP providers](#sdk3-support-cng-ksp)

## Linux support<a name="sdk3-linux"></a>
+ Amazon Linux
+ Amazon Linux 2
+ CentOS 6\.10\+ 2
+ CentOS 7\.3\+
+ CentOS 8 1,4
+ Red Hat Enterprise Linux \(RHEL\) 6\.10\+ 2
+ Red Hat Enterprise Linux \(RHEL\) 7\.3\+
+ Red Hat Enterprise Linux \(RHEL\) 8 1
+ Ubuntu 16\.04 LTS 3
+ Ubuntu 18\.04 LTS 1

\[1\] No support for OpenSSL Dynamic Engine\. For more information, see [OpenSSL Dynamic Engine](#openssl-collapse)\.

\[2\] No support for Client SDK 3\.3\.0 and later\.

\[3\] SDK 3\.4 is the last supported release on Ubuntu 16\.04\.

\[4\] SDK 3\.4 is the last supported release on CentOS 8\.3\+\.

## Windows support<a name="sdk3-windows"></a>
+ Microsoft Windows Server 2012
+ Microsoft Windows Server 2012 R2
+ Microsoft Windows Server 2016
+ Microsoft Windows Server 2019

## Components support<a name="sdk3-support-components"></a>

### PKCS \#11 library<a name="sdk3-support-pkcs11"></a>

The PKCS \#11 library is a Linux only component that matches Linux base support\. For more information, see [Linux support](#sdk3-linux)\.

### JCE provider<a name="sdk3-support-jce"></a>

The JCE provider is a Linux only component that matches Linux base support\. For more information, see [Linux support](#sdk3-linux)\.
+ Requires OpenJDK 1\.8 

### OpenSSL Dynamic Engine<a name="sdk3-support-openssl"></a><a name="openssl-collapse"></a>

The OpenSSL Dynamic Engine is Linux only component that does *not* match Linux base support\. See the exclusions below\. 
+  Requires OpenSSL 1\.0\.2\[f\+\]

**Unsupported platforms: **
+ CentOS 8
+ Red Hat Enterprise Linux \(RHEL\) 8
+ Ubuntu 18\.04 LTS

These platforms ship with a version of OpenSSL incompatible with OpenSSL Dynamic Engine for Client SDK 3\. AWS CloudHSM supports these platforms with OpenSSL Dynamic Engine for Client SDK 5\.

### CNG and KSP providers<a name="sdk3-support-cng-ksp"></a>

The CNG and KSP providers is a Windows only component that matches Windows base support\. For more information, see [Windows support](#sdk3-windows)\.