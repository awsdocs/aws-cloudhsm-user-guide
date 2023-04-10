# Installing Client SDK 3 for PKCS \#11 library<a name="install-pkcs11-v3"></a>

## Prerequisites for Client SDK 3<a name="pkcs11-library-prerequisites"></a>

The PKCS \#11 library requires the AWS CloudHSM client\.

If you haven't installed and configured the AWS CloudHSM client, do that now by following the steps at [Install the client \(Linux\)](cmu-install-and-configure-client-linux.md)\. After you install and configure the client, use the following command to start it\. 

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
#### [ CentOS 7 ]

```
$ sudo service cloudhsm-client start
```

------
#### [ CentOS 8 ]

```
$ sudo service cloudhsm-client start
```

------
#### [ RHEL 7 ]

```
$ sudo service cloudhsm-client start
```

------
#### [ RHEL 8 ]

```
$ sudo service cloudhsm-client start
```

------
#### [ Ubuntu 16\.04 LTS ]

```
$ sudo service cloudhsm-client start
```

------
#### [ Ubuntu 18\.04 LTS ]

```
$ sudo service cloudhsm-client start
```

------

## Install the PKCS \#11 library for Client SDK 3<a name="install-pkcs11-library"></a>

The following command downloads and installs the PKCS \#11 library\.

------
#### [ Amazon Linux ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-client-pkcs11-latest.el6.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-client-pkcs11-latest.el6.x86_64.rpm
```

------
#### [ Amazon Linux 2 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

------
#### [ CentOS 7 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

------
#### [ CentOS 8 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-client-pkcs11-latest.el8.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-client-pkcs11-latest.el8.x86_64.rpm
```

------
#### [ RHEL 7 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

------
#### [ RHEL 8 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-client-pkcs11-latest.el8.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-client-pkcs11-latest.el8.x86_64.rpm
```

------
#### [ Ubuntu 16\.04 LTS ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Xenial/cloudhsm-client-pkcs11_latest_amd64.deb
```

```
$ sudo apt install ./cloudhsm-client-pkcs11_latest_amd64.deb
```

------
#### [ Ubuntu 18\.04 LTS ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Bionic/cloudhsm-client-pkcs11_latest_u18.04_amd64.deb
```

```
$ sudo apt install ./cloudhsm-client-pkcs11_latest_u18.04_amd64.deb
```

------
+ If the EC2 instance on which you installed the PKCS \#11 library has no other components from Client SDK 3 installed, you must bootstrap Client SDK 3\. You only have to do this once on each instance with a component from Client SDK 3\.
+ You can find the PKCS \#11 library files in the following locations:

  Linux binaries, configuration scripts, certificates, and log files:

  ```
  /opt/cloudhsm/lib
  ```