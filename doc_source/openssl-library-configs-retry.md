# Retry configuration for OpenSSL<a name="openssl-library-configs-retry"></a>

Retry configuration can be set to one of two modes: **Off** and **Standard Retry**\. By default, **Standard Retry** is the default mode for all SDKs, which means that when an first error is encountered most commands will be retried for specific non\-terminal return codes\. When retry mode is **Off** no retries will be attempted\.

## Set retry configuration to Off mode<a name="w13aac21c15c15b7b5"></a>

------
#### [ Linux ]

**To set retry configuration to off for Client SDK 5 on Linux**
+ You can use the following command to set retry configuration to off mode:

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn --default-retry-mode off
  ```

------
#### [ Windows ]

**To set retry configuration to off for Client SDK 5 on Windows**
+ You can use the following command to set retry configuration to off mode:

  ```
  C:\Program Files\Amazon\CloudHSM\bin\ .\configure-dyn.exe --default-retry-mode off
  ```

------