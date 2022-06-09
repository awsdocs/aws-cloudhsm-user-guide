# Microsoft SignTool with AWS CloudHSM step 1: Set up the prerequisites<a name="signtool-prereqs"></a>

To use Microsoft SignTool with AWS CloudHSM, you need the following:
+ An Amazon EC2 client instance running a Windows operating system\.
+ A certificate authority \(CA\), either self\-maintained or established by a third\-party provider\.
+ An active AWS CloudHSM cluster in the same virtual public cloud \(VPC\) as your EC2 instance\. The cluster must contain at least one HSM\.
+ A crypto user \(CU\) to own and manage keys in the AWS CloudHSM cluster\.
+ An unsigned file or executable\.
+ The Microsoft Windows Software Development Kit \(SDK\)\.

**To set up the prerequisites for using AWS CloudHSM with Windows SignTool**

1. Follow the instructions in the [Getting Started](getting-started.md) section of this guide to launch a Windows EC2 instance and an AWS CloudHSM cluster\.

1. If you would like to host your own Windows Server CA, follow steps 1 and 2 in [Configuring Windows Server as a Certificate Authority with AWS CloudHSM](win-ca-overview.md)\. Otherwise, continue to use your publically trusted third\-party CA\.

1. Download and install one of the following versions of the Microsoft Windows SDK on your Windows EC2 instance:
   + [Microsoft Windows SDK 10](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk)
   + [Microsoft Windows SDK 8\.1](https://developer.microsoft.com/en-us/windows/downloads/windows-8-1-sdk)
   + [Microsoft Windows SDK 7](https://www.microsoft.com/en-us/download/details.aspx?id=8279)

   The `SignTool` executable is part of the Windows SDK Signing Tools for Desktop Apps installation feature\. You can omit the other features to be installed if you don’t need them\. The default installation location is:

   ```
   C:\Program Files (x86)\Windows Kits\<SDK version>\bin\<version number>\<CPU architecture>\signtool.exe
   ```

You can now use the Microsoft Windows SDK, your AWS CloudHSM cluster, and your CA to [Create a Signing Certificate](signtool-csr.md)\.