# Install the KSP and CNG Providers for Windows<a name="ksp-library-install"></a>

The Cavium KSP and CNG providers are installed when you install the Windows AWS CloudHSM client\. You can install the client by following the steps at [Install the Client \(Windows\)](install-and-configure-client-win.md)\. You can choose the Cavium KSP when you add the AD CS role to your Windows Server\. See [Create Windows Server CA](win-ca-setup.md)\.

## Configure and Run the Windows AWS CloudHSM Client<a name="ksp-configure-client-windows"></a>

To start the Windows CloudHSM client, you must first satisfy the [Prerequisites](ksp-library-prereq.md)\. Then, update the configuration files that the providers use and start the client by completing the steps below\.\. You need to do these steps the first time you use the KSP and CNG providers and after you add or remove HSMs in your cluster\. This way, AWS CloudHSM is able to synchronize data and maintain consistency across all HSMs in the cluster\.

### Step 1: Stop the AWS CloudHSM Client<a name="ksp-stop-cloudhsm-client"></a>

Before you update the configuration files that the providers use, stop the AWS CloudHSM client\. If the client is already stopped, running the stop command has no effect\. 
+ For Windows client 1\.1\.2\+:

  ```
  C:\Program Files\Amazon\CloudHSM>net.exe stop AWSCloudHSMClient
  ```
+ For Windows clients 1\.1\.1 and older:

  Use **Ctrl**\+**C** in the command window where you started the AWS CloudHSM client\.

### Step 2: Update the AWS CloudHSM Configuration Files<a name="ksp-config-a"></a>

This step uses the `-a` parameter of the [Configure tool](configure-tool.md) to add the elastic network interface \(ENI\) IP address of one of the HSMs in the cluster to the configuration file\. 

```
c:\Program Files\Amazon\CloudHSM>configure.exe -a <HSM ENI IP>
```

To get the ENI IP address of an HSM in your cluster, navigate to the AWS CloudHSM console, choose **clusters**, and select the desired cluster\. You can also use the [DescribeClusters](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeClusters.html) operation, the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command, or the [Get\-HSM2Cluster](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-HSM2Cluster.html) PowerShell cmdlet\. Type only one ENI IP address\. It does not matter which ENI IP address you use\. 

### Step 3: Start the AWS CloudHSM Client<a name="ksp-start-cloudhsm-client"></a>

Next, start or restart the AWS CloudHSM client\. When the AWS CloudHSM client starts, it uses the ENI IP address in its configuration file to query the cluster\. Then it adds the ENI IP addresses of all HSMs in the cluster to the cluster information file\. 
+ For Windows client 1\.1\.2\+:

  ```
  C:\Program Files\Amazon\CloudHSM>net.exe start AWSCloudHSMClient
  ```
+ For Windows clients 1\.1\.1 and older:

  ```
  C:\Program Files\Amazon\CloudHSM>start "cloudhsm_client" cloudhsm_client.exe C:\ProgramData\Amazon\CloudHSM\data\cloudhsm_client.cfg
  ```

## Checking the KSP and CNG Providers<a name="ksp-check-providers"></a>

You can use either of the following commands to determine which providers are installed on your system\. The commands list the registered KSP and CNG providers\. The AWS CloudHSM client does not need to be running\. 

```
C:\Program Files\Amazon\CloudHSM>ksp_config.exe -enum
```

```
C:\Program Files\Amazon\CloudHSM>cng_config.exe -enum
```

Verify that the Cavium KSP and CNG providers are installed on your Windows Server EC2 instance\. If the CNG provider is missing, run the following command\. 

```
C:\Program Files\Amazon\CloudHSM>cng_config.exe -register
```

If the Cavium KSP is missing, run the following command\.

```
C:\Program Files\Amazon\CloudHSM>ksp_config.exe -register
```