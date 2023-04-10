# Install the OpenSSL Dynamic Engine for Client SDK 3<a name="openssl3-install-openssl-library"></a>

The following steps describe how to install and configure the AWS CloudHSM dynamic engine for OpenSSL\. For information about upgrading, see [Upgrading Client SDK 3](client-upgrade.md)\.

**To install and configure the OpenSSL engine**

1. Use the following commands to download and install the OpenSSL engine\.

------
#### [ Amazon Linux ]

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-client-dyn-latest.el6.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-client-dyn-latest.el6.x86_64.rpm
   ```

------
#### [ Amazon Linux 2 ]

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-dyn-latest.el7.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-client-dyn-latest.el7.x86_64.rpm
   ```

------
#### [ CentOS 6 ]

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-client-dyn-latest.el6.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-client-dyn-latest.el6.x86_64.rpm
   ```

------
#### [ CentOS 7 ]

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-dyn-latest.el7.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-client-dyn-latest.el7.x86_64.rpm
   ```

------
#### [ RHEL 6 ]

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-client-dyn-latest.el6.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-client-dyn-latest.el6.x86_64.rpm
   ```

------
#### [ RHEL 7 ]

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-dyn-latest.el7.x86_64.rpm
   ```

   ```
   $ sudo yum install ./cloudhsm-client-dyn-latest.el7.x86_64.rpm
   ```

------
#### [ Ubuntu 16\.04 LTS ]

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Xenial/cloudhsm-client-dyn_latest_amd64.deb
   ```

   ```
   $ sudo apt install ./cloudhsm-client-dyn_latest_amd64.deb
   ```

------

   The OpenSSL engine is installed at `/opt/cloudhsm/lib/libcloudhsm_openssl.so`\.

1. Use the following command to set an environment variable named `n3fips_password` that contains the credentials of a crypto user \(CU\)\. 

   ```
   $ export n3fips_password=<HSM user name>:<password>
   ```