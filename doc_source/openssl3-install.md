# OpenSSL Dynamic Engine Client SDK 3<a name="openssl3-install"></a>

## Prerequisites<a name="dyn3-prereqs"></a>

For information about platform support, see [Supported platforms for the client SDKs](client-supported-platforms.md)\.

Before you can use the AWS CloudHSM dynamic engine for OpenSSL, you need the AWS CloudHSM client\. 

The client is a daemon that establishes end\-to\-end encrypted communication with the HSMs in your cluster, and the OpenSSL engine communicates locally with the client\. To install and configure the AWS CloudHSM client, see [Install the client \(Linux\)](install-and-configure-client-linux.md)\. Then use the following command to start it\. 

------
#### [ Amazon Linux ]

```
$ sudo start cloudhsm-client
```

------
#### [ Amazon Linux 2 ]

```
$ sudo service cloudhsm-client start
```

------
#### [ CentOS 6 ]

```
$ sudo service start cloudhsm-client
```

------
#### [ CentOS 7 ]

```
$ sudo service cloudhsm-client start
```

------
#### [ RHEL 6 ]

```
$ sudo service start cloudhsm-client
```

------
#### [ RHEL 7 ]

```
$ sudo service cloudhsm-client start
```

------
#### [ Ubuntu 16\.04 LTS ]

```
$ sudo service cloudhsm-client start
```

------

## Install the OpenSSL Dynamic Engine for Client SDK 3<a name="install-openssl-library"></a>

The following steps describe how to install and configure the AWS CloudHSM dynamic engine for OpenSSL\. For information about upgrading, see [Upgrade your Client SDK 3](client-upgrade.md)\.

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

## Use the OpenSSL Dynamic Engine for Client SDK 3<a name="use-openssl-library"></a>

To use the AWS CloudHSM dynamic engine for OpenSSL from an OpenSSL\-integrated application, ensure that your application uses the OpenSSL dynamic engine named `cloudhsm`\. The shared library for the dynamic engine is located at `/opt/cloudhsm/lib/libcloudhsm_openssl.so`\.

To use the AWS CloudHSM dynamic engine for OpenSSL from the OpenSSL command line, use the `-engine` option to specify the OpenSSL dynamic engine named `cloudhsm`\. For example:

```
$ openssl s_server -cert server.crt -key server.key -engine cloudhsm
```