# Install the KSP and CNG Providers for Windows<a name="ksp-library-install"></a>

The Cavium KSP and CNG providers are installed when you install the Windows AWS CloudHSM client\.
+ For client installation instructions, see [Install the Client \(Windows\)](install-and-configure-client-win.md)\.
+ Before you can use the Windows CloudHSM client, you must satisfy the [Prerequisites](ksp-library-prereq.md)\.
+ You can choose the Cavium KSP when add the AD CS role to your Windows Server\. See [Create Windows Server CA](win-ca-setup.md)\.

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