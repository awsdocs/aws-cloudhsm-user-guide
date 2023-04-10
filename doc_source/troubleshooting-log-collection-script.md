# Retrieving client configuration logs<a name="troubleshooting-log-collection-script"></a>

AWS CloudHSM offers tools for Client SDK 3 and Client SDK 5 to gather information about your environment for AWS Support to troubleshoot problems\. 

**Topics**
+ [Client SDK 5 support tool](#support-tool-sdk5)
+ [Client SDK 3 support tool](#support-tool-sdk3)

## Client SDK 5 support tool<a name="support-tool-sdk5"></a>

The script extracts the following information:
+ The configuration file for the Client SDK 5 component
+ Available log files
+ Current version of the operating system
+ Package information

### Running the client tool for Client SDK 5<a name="running-sdk5"></a>

Client SDK 5 includes a client support tool for each component, but all tools function the same\. Run the tool to create an output file with all the gathered information\. 

The tools use a syntax like this: 

```
[ pkcs11 | dyn | jce ]_info
```

For example, to gather information for support from a Linux host running PKCS \#11 library and have the system write to the default directory, you would run this command: 

```
/opt/cloudhsm/bin/pkcs11_info
```

The tool creates the output file inside the `/tmp` directory\.

------
#### [ PKCS \#11 library ]

**To gather support data for PKCS \#11 library on Linux**
+  Use the support tool to gather data\. 

  ```
  /opt/cloudhsm/bin/pkcs11_info
  ```

**To gather support data for PKCS \#11 library on Windows**
+  Use the support tool to gather data\. 

  ```
  C:\Program Files\Amazon\CloudHSM\bin\pkcs11_info.exe
  ```

------
#### [ OpenSSL Dynamic Engine ]

**To gather support data for OpenSSL Dynamic Engine on Linux**
+  Use the support tool to gather data\. 

  ```
  /opt/cloudhsm/bin/dyn_info
  ```

------
#### [ JCE provider ]

**To gather support data for JCE provider on Linux**
+  Use the support tool to gather data\. 

  ```
  /opt/cloudhsm/bin/jce_info
  ```

**To gather support data for JCE provider on Windows**
+  Use the support tool to gather data\. 

  ```
  C:\Program Files\Amazon\CloudHSM\bin\jce_info.exe
  ```

------

## Client SDK 3 support tool<a name="support-tool-sdk3"></a>

The script extracts the following information:
+ Operating system and its current version
+ Client configuration information from `cloudhsm_client.cfg`, `cloudhsm_mgmt_util.cfg`, and `application.cfg` files
+ Client logs from the location specific to the platform
+ Cluster and HSM information by using cloudhsm\_mgmt\_util
+ OpenSSL information
+ Current client and build version
+ Installer version

### Running the client tool for Client SDK 3<a name="running-script"></a>

The script creates an output file with all the gathered information\. The script creates the output file inside the `/tmp` directory\.

**Linux**: `/opt/cloudhsm/bin/client_info`

**Windows**: `C:\Program Files\Amazon\CloudHSM\client_info`

**Warning**  
This script has a known issue for Client SDK 3 versions 3\.1\.0 through 3\.3\.1\. We strongly recommend you upgrade to version 3\.3\.2 which includes a fix for this issue\. Please refer to the [Known Issues](https://docs.aws.amazon.com/cloudhsm/latest/userguide/ki-all.html#ki-all-9) page for more information before using this tool\.