# Installing the OpenSSL Dynamic Engine<a name="openssl5-install"></a>

**To install and configure the OpenSSL Dynamic Engine**

1. Use the following commands to download and install the OpenSSL engine\.

------
#### [ Amazon Linux ]

   Install OpenSSL Dynamic Engine for Amazon Linux on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-dyn-latest.el6.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-dyn-latest.el6.x86_64.rpm
   ```

------
#### [ Amazon Linux 2 ]

   Install OpenSSL Dynamic Engine for Amazon Linux 2 on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-dyn-latest.el7.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-dyn-latest.el7.x86_64.rpm
   ```

   Install OpenSSL Dynamic Engine for Amazon Linux 2 on ARM64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-dyn-latest.el7.aarch64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-dyn-latest.el7.aarch64.rpm
   ```

------
#### [ CentOS 7 ]

   Install OpenSSL Dynamic Engine for CentOS 7 on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-dyn-latest.el7.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-dyn-latest.el7.x86_64.rpm
   ```

------
#### [ CentOS 8 ]

   Install OpenSSL Dynamic Engine for CentOS 8 on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-dyn-latest.el8.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-dyn-latest.el8.x86_64.rpm
   ```

------
#### [ RHEL 7 ]

   Install OpenSSL Dynamic Engine for RHEL 7 on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-dyn-latest.el7.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-dyn-latest.el7.x86_64.rpm
   ```

------
#### [ RHEL 8 ]

   Install OpenSSL Dynamic Engine for RHEL 8 on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-dyn-latest.el8.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-dyn-latest.el8.x86_64.rpm
   ```

------
#### [ Ubuntu 18\.04 LTS ]

   Install OpenSSL Dynamic Engine for Ubuntu 18\.04 LTS on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Bionic/cloudhsm-dyn_latest_u18.04_amd64.deb
   ```

   ```
   $ sudo apt install ./cloudhsm-dyn_latest_u18.04_amd64.deb
   ```

------
#### [ Ubuntu 20\.04 LTS ]

   Install OpenSSL Dynamic Engine for Ubuntu 20\.04 LTS LTS on X86\_64 architecture:

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Focal/cloudhsm-dyn_latest_u20.04_amd64.deb
   ```

   ```
   $ sudo apt install ./cloudhsm-dyn_latest_u20.04_amd64.deb
   ```

------

   You have installed the shared library for the dynamic engine at `/opt/cloudhsm/lib/libcloudhsm_openssl_engine.so`\.

1. Set an environment variable with the credentials of a crypto user \(CU\)\. For information about creating CUs, see [Using CMU to manage users](cli-users.md)\.

   ```
   $ export CLOUDHSM_PIN=<HSM user name>:<password>
   ```
**Note**  
Client SDK 5 introduces the `CLOUDHSM_PIN` environment variable for storing the credentials of the CU\. In Client SDK 3 you store the CU credentials in the `n3fips_password` environment variable\. Client SDK 5 supports both environment variables, but we recommend using `CLOUDHSM_PIN`\.

1. Connect your installation of OpenSSL Dynamic Engine to the cluster\. For more information, see [Connect to the Cluster](cluster-connect.md)\.

1. Bootstrap the Client SDK 5\. For more information, see [Bootstrap the Client SDK](cluster-connect.md#connect-how-to)\.

## Verify the OpenSSL Dynamic Engine for Client SDK 5<a name="verify-dyn-5"></a>

Use the following command to verify your installation of OpenSSL Dynamic Engine\.

```
$ openssl engine -t cloudhsm
```

The following output verifies your configuration:

```
(cloudhsm) CloudHSM OpenSSL Engine
     [ available ]
```