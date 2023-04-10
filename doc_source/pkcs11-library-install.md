# Install Client SDK 5 for PKCS \#11 library<a name="pkcs11-library-install"></a>

This topic provides instructions for installing the latest version of the PKCS \#11 library from the Client SDK 5 version series\. For more information about the Client SDK or PKCS \#11 library, see [Using the Client SDK](use-hsm.md) and [PKCS \#11 library](pkcs11-library.md)\.

## Installation<a name="install-pkcs11-library-4"></a>

With Client SDK 5, you are not required to install or run a client daemon\. 

To run a single HSM cluster with Client SDK 5, you must first manage client key durability settings by setting `disable_key_availability_check` to `True`\. For more information, see [Key Synchronization](manage-key-sync.md) and [Client SDK 5 Configure Tool](configure-sdk-5.md)\. 

For more information about the PKCS \#11 library in Client SDK 5, see [PKCS \#11 library](pkcs11-library.md)\.

**To install and configure the PKCS \#11 library**

1. Use the following commands to download and install the PKCS \#11 library\.

------
#### [ Amazon Linux ]

   Install the PKCS \#11 library for Amazon Linux on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-pkcs11-latest.el6.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-pkcs11-latest.el6.x86_64.rpm
   ```

------
#### [ Amazon Linux 2 ]

   Install the PKCS \#11 library for Amazon Linux 2 on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-pkcs11-latest.el7.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-pkcs11-latest.el7.x86_64.rpm
   ```

   Install the PKCS \#11 library for Amazon Linux 2 on ARM64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-pkcs11-latest.el7.aarch64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-pkcs11-latest.el7.aarch64.rpm
   ```

------
#### [ CentOS 7\.8\+ ]

   Install the PKCS \#11 library for CentOS 7\.8\+ on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-pkcs11-latest.el7.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-pkcs11-latest.el7.x86_64.rpm
   ```

------
#### [ CentOS 8\.3\+ ]

   Install the PKCS \#11 library for CentOS 8\.3\+ on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-pkcs11-latest.el8.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-pkcs11-latest.el8.x86_64.rpm
   ```

------
#### [ RHEL 7\.9\+ ]

   Install the PKCS \#11 library for RHEL 7\.9\+ on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-pkcs11-latest.el7.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-pkcs11-latest.el7.x86_64.rpm
   ```

------
#### [ RHEL 8\.3\+ ]

   Install the PKCS \#11 library for RHEL 8\.3\+ on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-pkcs11-latest.el8.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-pkcs11-latest.el8.x86_64.rpm
   ```

------
#### [ Ubuntu 18\.04 LTS ]

   Install the PKCS \#11 library for Ubuntu 18\.04 LTS on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Bionic/cloudhsm-pkcs11_latest_u18.04_amd64.deb
   ```

   ```
   $ sudo apt install ./cloudhsm-pkcs11_latest_u18.04_amd64.deb
   ```

------
#### [ Ubuntu 20\.04 LTS ]

   Install the PKCS \#11 library for Ubuntu 20\.04 LTS on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Focal/cloudhsm-pkcs11_latest_u20.04_amd64.deb
   ```

   ```
   $ sudo apt install ./cloudhsm-pkcs11_latest_u20.04_amd64.deb
   ```

------
#### [ Windows Server 2016 ]

   Install the PKCS \#11 library for Windows Server 2016 on X86\_64 architecture:

   1. Download [PKCS \#11 library for Client SDK 5](https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMPKCS11-latest.msi)\.

   1. Run the PKCS \#11 library installer \(**AWSCloudHSMPKCS11\-latest\.msi**\) with Windows administrative privilege\.

------
#### [ Windows Server 2019 ]

   Install the PKCS \#11 library for Windows Server 2019 on X86\_64 architecture:

   1. Download [PKCS \#11 library for Client SDK 5](https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMPKCS11-latest.msi)\.

   1. Run the PKCS \#11 library installer \(**AWSCloudHSMPKCS11\-latest\.msi**\) with Windows administrative privilege\.

------

1. You must bootstrap Client SDK 5\. For more information about bootstrapping, see [Bootstrapping the Client SDK](cluster-connect.md)\.

1. You can find the PKCS \#11 library files in the following locations:
   + Linux binaries, configuration scripts, and log files:

     ```
     /opt/cloudhsm
     ```

     Windows binaries:

     ```
     C:\ProgramFiles\Amazon\CloudHSM
     ```

     Windows configuration scripts and log files:

     ```
     C:\ProgramData\Amazon\CloudHSM
     ```