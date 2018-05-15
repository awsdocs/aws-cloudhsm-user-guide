# Set Up the Prerequisites to Configure AD CS to use the Cavium KSP<a name="win-ca-prerequisites"></a>

Before you can use the Cavium KSP provider to create and store the private key for a Windows certificate authority \(CA\), you must install the Windows AWS CloudHSM client\. For more information, see [Install the Client \(Windows\)](install-and-configure-client-win.md)\. Next create a crypto user \(CU\) and set the `n3fips_password` and `n3fips_partition` environment variables as discussed in [Windows AWS CloudHSM Prerequisites](ksp-library-prereq.md)\. Then use the following command to start the client\. 

```
c:\Program Files\Amazon\CloudHSM>cloudhsm_client.exe C:\ProgramData\Amazon\CloudHSM\data\cloudhsm_client.cfg
```