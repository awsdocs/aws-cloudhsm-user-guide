# Prerequisites for OpenSSL Dynamic Engine with Client SDK 3<a name="openssl3-install-dyn3-prereqs"></a>

For information about platform support, see [Client SDK 5 supported platforms](client-supported-platforms.md)\.

Before you can use the AWS CloudHSM dynamic engine for OpenSSL, you need the AWS CloudHSM client\. 

The client is a daemon that establishes end\-to\-end encrypted communication with the HSMs in your cluster, and the OpenSSL engine communicates locally with the client\. To install and configure the AWS CloudHSM client, see [Install the client \(Linux\)](cmu-install-and-configure-client-linux.md)\. Then use the following command to start it\. 

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