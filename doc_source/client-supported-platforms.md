# Client SDK 5 supported platforms<a name="client-supported-platforms"></a>

Base support is different for each version of the AWS CloudHSM Client SDK\. Platform support for components in an SDK typically matches base support, but not always\. To determine platform support for a given component, first make sure the platform you want appears in the base section for the SDK, then check for an exclusions or any other pertinent information in the component section\.

AWS CloudHSM supports only 64\-bit operating systems\.

Platform support changes over time\. Earlier versions of the CloudHSM Client SDK may not support all the operating systems listed here\. Use release notes to determine the operating system support for previous versions of the CloudHSM Client SDK\. For more information, see [Downloads for AWS CloudHSM Client SDK](client-history.md)\.

For supported platforms for the previous Client SDK, see [Client SDK 3 supported platforms](sdk3-support.md)

## Client SDK 5 platform support<a name="sdk8-support"></a>

Client SDK 5 does not require a client daemon\. 

### Linux support for Client SDK 5<a name="sdk8-linux"></a>


| Supported platforms | X86\_64 Architecture | ARM architecture | 
| --- | --- | --- | 
| Amazon Linux |  Yes  |  No  | 
| Amazon Linux 2 | Yes | Yes | 
| CentOS 7\.8\+ | Yes | No | 
| CentOS 8\.3\+ | Yes | No | 
| Red Hat Enterprise Linux \(RHEL\) 7\.8\+ | Yes | No | 
| Red Hat Enterprise Linux \(RHEL\) 8\.3\+ | Yes | No | 
| Ubuntu 18\.04 LTS | Yes | No | 
| Ubuntu 20\.04 LTS | Yes | No | 

\[1\] SDK 5\.4\.1 is the last supported release on CentOS 8\.3\+\.

### Windows support for Client SDK 5<a name="sdk8-windows"></a>
+ Microsoft Windows Server 2016
+ Microsoft Windows Server 2019

### Serverless support for Client SDK 5<a name="sdk8-serverless"></a>
+ AWS Lambda
+ Docker/ECS

### Components support<a name="sdk8-support-components"></a>

#### PKCS \#11 library<a name="sdk8-support-pkcs11"></a>

The PKCS \#11 library is a cross\-platform component that matches Linux and Windows Client SDK 5 base support\. For more information, see [Linux support for Client SDK 5](#sdk8-linux) and [Windows support for Client SDK 5](#sdk8-windows)\.

#### OpenSSL Dynamic Engine<a name="sdk5-support-openssl"></a><a name="openssl5-collapse"></a>

The OpenSSL Dynamic Engine is a Linux only component that requires OpenSSL 1\.0\.2 or 1\.1\.1\.

#### JCE provider<a name="sdk5-support-jce"></a>

The JCE provider is a Java SDK which is compatible with OpenJDK 8 and OpenJDK11 on all supported platforms\.