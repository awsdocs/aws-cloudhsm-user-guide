# Windows AWS CloudHSM prerequisites<a name="ksp-library-prereq"></a>

Before you can start the Windows AWS CloudHSM client and use the KSP and CNG providers, you must set the login credentials for the HSM on your system\. You can set credentials through either Windows Credentials Manager or system environment variable\. We recommend you use Windows Credential Manager for storing credentials\. This option is available with AWS CloudHSM client version 2\.0\.4 and later\. Using environment variable is easier to set up, but less secure than using Windows Credential Manager\.

## Windows Credential Manager<a name="wcm"></a>

You can use either the `set_cloudhsm_credentials` utility or the Windows Credentials Manager interface\.
+ **Using the `set_cloudhsm_credentials` utility**:

  The `set_cloudhsm_credentials` utility is included in your Windows installer\. You can use this utility to conveniently pass HSM login credentials to Windows Credential Manager\. If you want to compile this utility from source, you can use the Python code that is included in the installer\.

  1. Go to the `C:\Program Files\Amazon\CloudHSM\tools\` folder\.

  1. Run the `set_cloudhsm_credentials.exe` file with the CU username and password parameters\.

     ```
     set_cloudhsm_credentials.exe --username <cu-user> --password <cu-pwd>
     ```
+ **Using the Credential Manager interface**:

  You can use the Credential Manager interface to manually manage your credentials\.

  1. To open Credential Manager, type `credential manager` in the search box on the taskbar and select **Credential Manager**\.

  1. Select **Windows Credentials** to manage Windows credentials\.

  1. Select **Add a generic credential** and fill out the details as follows:
     + In **Internet or Network Address**, enter the target name as `cloudhsm_client`\.
     + In **Username** and **Password** enter the CU credentials\.
     + Click **OK**\.

## System environment variables<a name="enviorn-var"></a>

You can set system environment variables that identify an HSM and a [crypto user](manage-hsm-users-cmu.md#crypto-user-cmu) \(CU\) for your Windows application\. You can use the [setx command](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/setx) to set system environment variables, or set permanent system environment variables [programmatically](https://msdn.microsoft.com/en-us/library/system.environment.setenvironmentvariable(v=vs.110).aspx) or in the **Advanced** tab of the Windows **System Properties** Control Panel\. 

**Warning**  
When you set credentials through system environment variables, the password is available in plaintext on a user’s system\. To overcome this problem, use Windows Credential Manager\.

Set the following system environment variables:

**`n3fips_password=CU-username:CU-password`**  
Identifies a [crypto user](manage-hsm-users-cmu.md#crypto-user-cmu) \(CU\) in the HSM and provides all required login information\. Your application authenticates and runs as this CU\. The application has the permissions of this CU and can view and manage only the keys that the CU owns and shares\. To create a new CU, use [createUser](cloudhsm_mgmt_util-createUser.md)\. To find existing CUs, use [listUsers](cloudhsm_mgmt_util-listUsers.md)\.  
For example:  

```
setx /m n3fips_password test_user:password123
```