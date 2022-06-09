# Client SDK 5 platform support<a name="sdk8-support"></a>

Client SDK 5 does not require a client daemon\. 

**Contents**
+ [Linux support for Client SDK 5](#sdk8-linux)
+ [Windows support for Client SDK 5](#sdk8-windows)
+ [Serverless support for Client SDK 5](#sdk8-serverless)
+ [Components support](#sdk8-support-components)
  + [PKCS \#11 library](#sdk8-support-pkcs11)
  + [OpenSSL Dynamic Engine](#sdk5-support-openssl)
  + [JCE provider](#sdk5-support-jce)

## Linux support for Client SDK 5<a name="sdk8-linux"></a>
+ Amazon Linux
+ Amazon Linux 2
+ CentOS 7\.8\+
+ CentOS 8\.3\+ 1
+ Red Hat Enterprise Linux \(RHEL\) 7\.8\+
+ Red Hat Enterprise Linux \(RHEL\) 8\.3\+ 
+ Ubuntu 18\.04 LTS 

\[1\] SDK 5\.4\.1 is the last supported release on CentOS 8\.3\+\.

## Windows support for Client SDK 5<a name="sdk8-windows"></a>
+ Microsoft Windows Server 2016
+ Microsoft Windows Server 2019

## Serverless support for Client SDK 5<a name="sdk8-serverless"></a>
+ AWS Lambda
+ Docker/ECS

## Components support<a name="sdk8-support-components"></a>

### PKCS \#11 library<a name="sdk8-support-pkcs11"></a>

The PKCS \#11 library is a cross\-platform component that matches Linux and Windows Client SDK 5 base support\. For more information, see [Linux support for Client SDK 5](#sdk8-linux) and [Windows support for Client SDK 5](#sdk8-windows)\.

### OpenSSL Dynamic Engine<a name="sdk5-support-openssl"></a><a name="openssl5-collapse"></a>

The OpenSSL Dynamic Engine is a Linux only component that requires OpenSSL 1\.0\.2 or 1\.1\.1\.

### JCE provider<a name="sdk5-support-jce"></a>

The JCE provider is a Java SDK which is compatible with OpenJDK 8 and OpenJDK11 on all supported platforms\.