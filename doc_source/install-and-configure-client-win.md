# Install and Configure the AWS CloudHSM Client \(Windows\)<a name="install-and-configure-client-win"></a>

To interact with an HSM in your AWS CloudHSM cluster, you need the AWS CloudHSM client software for Windows\. You should install it on the Windows Server instance that you created previously\. You can also install a client if you are using Linux\. For more information, see [Install and Configure the AWS CloudHSM Client \(Linux\)](install-and-configure-client-linux.md)\. 

**To install \(or update\) the client and command line tools**

1. Connect to your Windows Server instance\.

1. Download the [AWSCloudHSMClient\.msi installer](https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMClient-latest.msi)\. 

1. Go to your download location and run the **AWSCloudHSMClient\.msi** installer\. Follow the installer instructions\. 
**Important**  
You must run the installer with administrative privileges\. Open a command window as an administrator, navigate to the `C:\Program Files\Amazon\CloudHSM` directory, and run `AWSCloudHSMClient.exe`\. 
**Note**  
You can uninstall the client by running the installer again and following the directions\.

1. Choose **Close** after the installer dialog has finished\.
**Note**  
The installer automatically registers the KSP and CNG providers\.

   The installer copies the executable files into the `C:\Program Files\Amazon\CloudHSM` folder:
   + cloudhsm\_client
   + cloudhsm\_mgmt\_util
   + cng\_config
   + configure
   + key\_mgmt\_util
   + ksp\_config
   + pkpspeed\_blocking\.exe

   Certificates and keys are installed in the `C:\ProgramData\Amazon\CloudHSM` folder:
   + client\.crt
   + client\.key

   Configuration files are copied into the `C:\ProgramData\Amazon\CloudHSM\data` folder:
   + application\.cfg
   + cloudhsm\_client\.cfg
   + cloudhsm\_mgmt\_util\.cfg

1. Copy your self\-signed issuing certificate—[the one that you used to sign the cluster certificate](initialize-cluster.md#sign-csr)—to the `C:\ProgramData\Amazon\CloudHSM` folder\. 

1. Run the following command to update your configuration files:

   ```
   c:\Program Files\Amazon\CloudHSM>configure.exe -a <HSM IP address>
   ```

1. Go to [Activate the Cluster](activate-cluster.md)\.