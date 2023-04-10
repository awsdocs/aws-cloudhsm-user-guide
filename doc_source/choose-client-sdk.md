# Previous Client SDK versions<a name="choose-client-sdk"></a>

 AWS CloudHSM includes two major Client SDK versions: 
+ Client SDK 5: This is our latest and default Client SDK\. For information on the benefits and advantages it provides, see [Benefits of Client SDK 5](client-sdk-5-benefits.md)\.
+ Client SDK 3: This is our older Client SDK\. It includes a full set of components for platform and language\-based applications compatibility and management tools\.

Client SDK 3 documentation is listed in this section\.

To download, see [Downloads for AWS CloudHSM Client SDK](client-history.md)\.

## Check your client SDK version<a name="check-client_version"></a>

------
#### [ Amazon Linux ]

Use the following command:

```
rpm -qa | grep ^cloudhsm
```

------
#### [ Amazon Linux 2 ]

Use the following command:

```
rpm -qa | grep ^cloudhsm
```

------
#### [ CentOS 6 ]

Use the following command:

```
rpm -qa | grep ^cloudhsm
```

------
#### [ CentOS 7 ]

Use the following command:

```
rpm -qa | grep ^cloudhsm
```

------
#### [ CentOS 8 ]

Use the following command:

```
rpm -qa | grep ^cloudhsm
```

------
#### [ RHEL 6 ]

Use the following command:

```
rpm -qa | grep ^cloudhsm
```

------
#### [ RHEL 7 ]

Use the following command:

```
rpm -qa | grep ^cloudhsm
```

------
#### [ RHEL 8 ]

Use the following command:

```
rpm -qa | grep ^cloudhsm
```

------
#### [ Ubuntu 16\.04 LTS ]

Use the following command:

```
apt list --installed | grep ^cloudhsm
```

------
#### [ Ubuntu 18\.04 LTS ]

Use the following command:

```
apt list --installed | grep ^cloudhsm
```

------
#### [ Ubuntu 20\.04 LTS ]

Use the following command:

```
apt list --installed | grep ^cloudhsm
```

------
#### [ Windows Server ]

Use the following command:

```
wmic product get name,version
```

------

## Client SDK component comparison<a name="sdk-compare"></a>

In addition to the command\-line tools, Client SDK 3 contains components that enable off\-loading cryptographic operations to the HSM from various platform or language\-based applications\. Client SDK 5 does not have parity with Client SDK 3 at this time, but parity is the goal\. The following table compares component availability in Client SDK 3 and Client SDK 5\.


| Component | Client SDK 5 | Client SDK 3 | 
| --- | --- | --- | 
| PKCS \#11 library |  Yes  |  Yes  | 
| JCE provider | Yes | Yes | 
| OpenSSL Dynamic Engine | Yes |  Yes  | 
| CNG and KSP providers |  | Yes | 
| CloudHSM Management Utility |  | Yes1 | 
| Key management utility |  | Yes | 
| Configure tool | Yes | Yes | 

\[1\] To manage HSM users with Client SDK 5, you must use a standalone version of CloudHSM CLI or the CMU\. For more information, see [Managing HSM users with CloudHSM CLI](manage-hsm-users-chsm-cli.md) and [Managing HSM users with CloudHSM Management Utility \(CMU\)](manage-hsm-users-cmu.md)\.

**Topics**
+ [Check your client SDK version](#check-client_version)
+ [Client SDK component comparison](#sdk-compare)
+ [Client SDK 3 supported platforms](sdk3-support.md)
+ [Upgrading Client SDK 3 on Linux](client-upgrade.md)
+ [PKCS \#11 library for Client SDK 3](pkcs11-v3-library.md)
+ [Installing Client SDK 3 for OpenSSL Dynamic Engine](openssl3-install.md)
+ [Client SDK 3 for JCE provider](java-library_3.md)