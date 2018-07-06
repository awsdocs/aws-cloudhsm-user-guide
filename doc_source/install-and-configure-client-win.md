# Install and Configure the AWS CloudHSM Client \(Windows\)<a name="install-and-configure-client-win"></a>

To interact with an HSM in your AWS CloudHSM cluster, you need the AWS CloudHSM client software for Windows\. You should install it on the Windows Server instance that you created previously\. You can also install a client if you are using Linux\. For more information, see [Install and Configure the AWS CloudHSM Client \(Linux\)](install-and-configure-client-linux.md)\. 

**To install \(or update\) the client and command line tools**

1. Connect to your Windows Server instance\.

1. Download the [AWSCloudHSMClient\.msi installer](https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMClient-1.1.0.msi)\. 

1. Go to your download location and run the **AWSCloudHSMClient\.msi** installer\. Follow the installer instructions\. 
**Important**  
You must run the installer with administrative privileges\.

   The installer automatically registers the Cryptography API: Next Generation \(CNG\) key storage providers \(KSPs\) for AWS CloudHSM\. To uninstall the AWS CloudHSM client software for Windows, run the installer again and following the instructions\.

1. Choose **Close** after the installer has finished\.

   The installer copies the following executable files into the `C:\Program Files\Amazon\CloudHSM` folder:
   + `cloudhsm_client.exe`
   + `cloudhsm_mgmt_util.exe`
   + `cng_config.exe`
   + `configure.exe`
   + `key_mgmt_util.exe`
   + `ksp_config.exe`
   + `pkpspeed_blocking.exe`

   The installer copies the following certificate and key files into the `C:\ProgramData\Amazon\CloudHSM` folder:
   + `client.crt`
   + `client.key`

   The installer copies the following configuration files into the `C:\ProgramData\Amazon\CloudHSM\data` folder:
   + `application.cfg`
   + `cloudhsm_client.cfg`
   + `cloudhsm_mgmt_util.cfg`

1. Copy your self\-signed issuing certificate—[the one that you used to sign the cluster certificate](initialize-cluster.md#sign-csr)—to the `C:\ProgramData\Amazon\CloudHSM` folder\. 

1. Run the following command to update your configuration files:

   ```
   c:\Program Files\Amazon\CloudHSM>configure.exe -a <HSM IP address>
   ```

1. Go to [Activate the Cluster](activate-cluster.md)\.