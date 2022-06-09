# Choose a Client SDK version to work with AWS CloudHSM<a name="choose-client-sdk"></a>

 AWS CloudHSM includes two major Client SDK versions: 
+ Client SDK 3, which includes a full set of components for platform and language\-based applications compatibility and management tools\.
+ Client SDK 5, which does not require a client daemon, and offers true cross\-platform support for Windows and Linux\.

To download, see [Download AWS CloudHSM Client SDK](client-history.md)\.

## Client SDK comparison<a name="sdk-compare"></a>

In addition to the command\-line tools, Client SDK 3 contains components that enable off\-loading cryptographic operations to the HSM from various platform or language\-based applications\. Client SDK 5 does not have parity with Client SDK 3 at this time, but parity is the goal\. The following table compares component availability in Client SDK 3 and Client SDK 5\.


| Component | Client SDK 3 | Client SDK 5 | 
| --- | --- | --- | 
| PKCS \#11 library |  Yes  |  Yes  | 
| JCE provider | Yes | Yes | 
| OpenSSL Dynamic Engine | Yes |  Yes  | 
| CNG and KSP providers | Yes |  | 
| CloudHSM Management Utility | Yes1 |  | 
| Key management utility | Yes |  | 
| Configure tool | Yes | Yes | 

\[1\] To manage HSM users with Client SDK 5, AWS CloudHSM offers a standalone version of CMU\. For more information, see [Download CloudHSM Management Utility](cli-users.md#get-cli-users) and [How to manage HSM users with CMU](cli-users.md#manage-users)\.

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
#### [ Windows Server ]

Use the following command:

```
wmic product get name,version
```

------