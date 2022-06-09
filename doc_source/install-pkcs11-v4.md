# Install PKCS \#11 library Client SDK 5<a name="install-pkcs11-v4"></a>

## Install the PKCS \#11 library for Client SDK 5<a name="install-pkcs11-library-4"></a>

With Client SDK 5, you are not required to install or run a client daemon\. 

To run a single HSM cluster with Client SDK 5, you must first manage client key durability settings by setting `disable_key_availability_check` to `True`\. For more information, see [Key Synchronization](manage-key-sync.md) and [Client SDK 5 Configure Tool](configure-sdk-5.md)\. 

For more information about the PKCS \#11 library in Client SDK 5, see [PKCS \#11 library](pkcs11-library.md)\.
+ Download and install the PKCS \#11 library\.

------
#### [ Amazon Linux ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-pkcs11-latest.el6.x86_64.rpm
  ```

  ```
  $ sudo yum install ./cloudhsm-pkcs11-latest.el6.x86_64.rpm
  ```

------
#### [ Amazon Linux 2 ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-pkcs11-latest.el7.x86_64.rpm
  ```

  ```
  $ sudo yum install ./cloudhsm-pkcs11-latest.el7.x86_64.rpm
  ```

------
#### [ CentOS 7\.8\+ ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-pkcs11-latest.el7.x86_64.rpm
  ```

  ```
  $ sudo yum install ./cloudhsm-pkcs11-latest.el7.x86_64.rpm
  ```

------
#### [ CentOS 8\.3\+ ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-pkcs11-latest.el8.x86_64.rpm
  ```

  ```
  $ sudo yum install ./cloudhsm-pkcs11-latest.el8.x86_64.rpm
  ```

------
#### [ RHEL 7\.8\+ ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-pkcs11-latest.el7.x86_64.rpm
  ```

  ```
  $ sudo yum install ./cloudhsm-pkcs11-latest.el7.x86_64.rpm
  ```

------
#### [ RHEL 8\.3\+ ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-pkcs11-latest.el8.x86_64.rpm
  ```

  ```
  $ sudo yum install ./cloudhsm-pkcs11-latest.el8.x86_64.rpm
  ```

------
#### [ Ubuntu 18\.04 LTS ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Bionic/cloudhsm-pkcs11_latest_u18.04_amd64.deb
  ```

  ```
  $ sudo apt install ./cloudhsm-pkcs11_latest_u18.04_amd64.deb
  ```

------
#### [ Windows Server 2016 ]

  1. Download [PKCS \#11 library for Client SDK 5](https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMPKCS11-latest.msi)\.

  1. Run the PKCS \#11 library installer \(**AWSCloudHSMPKCS11\-latest\.msi**\) with Windows administrative privilege\.

------
#### [ Windows Server 2019 ]

  1. Download [PKCS \#11 library for Client SDK 5](https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMPKCS11-latest.msi)\.

  1. Run the PKCS \#11 library installer \(**AWSCloudHSMPKCS11\-latest\.msi**\) with Windows administrative privilege\.

------
+ You must bootstrap Client SDK 5\. For more information about bootstrapping, see [Bootstrapping the Client SDK](cluster-connect.md)\.
+ You can find the PKCS \#11 library files in the following locations:

  Linux binaries, configuration scripts, and log files:

  ```
  /opt/cloudhsm/lib
  ```

  Windows binaries:

  ```
  C:\ProgramFiles\Amazon\CloudHSM
  ```

  Windows configuration scripts and log files:

  ```
  C:\ProgramData\Amazon\CloudHSM
  ```