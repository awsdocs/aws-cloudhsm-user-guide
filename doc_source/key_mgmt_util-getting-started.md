# Getting started with key\_mgmt\_util<a name="key_mgmt_util-getting-started"></a>

AWS CloudHSM includes two command line tools with the [AWS CloudHSM client software](kmu-install-and-configure-client-linux.md#kmu-install-client)\. The [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-reference.md) tool includes commands to manage HSM users\. The [key\_mgmt\_util](key_mgmt_util-reference.md) tool includes commands to manage keys\. To get started with the key\_mgmt\_util command line tool, see the following topics\. 

**Topics**
+ [Set up key\_mgmt\_util](#key_mgmt_util-setup)
+ [Basic usage of key\_mgmt\_util](#key_mgmt_util-basics)

If you encounter an error message or unexpected outcome for a command, see the [Troubleshooting AWS CloudHSM](troubleshooting.md) topics for help\. For details about the key\_mgmt\_util commands, see [key\_mgmt\_util command reference](key_mgmt_util-reference.md)\. 

## Set up key\_mgmt\_util<a name="key_mgmt_util-setup"></a>

Complete the following setup before you use key\_mgmt\_util\.

### Start the AWS CloudHSM client<a name="key_mgmt_util-start-cloudhsm-client"></a>

Before you use key\_mgmt\_util, you must start the AWS CloudHSM client\. The client is a daemon that establishes end\-to\-end encrypted communication with the HSMs in your cluster\. The key\_mgmt\_util tool uses the client connection to communicate with the HSMs in your cluster\. Without it, key\_mgmt\_util doesn't work\. 

**To start the AWS CloudHSM client**  
Use the following command to start the AWS CloudHSM client\.

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
#### [ Windows ]
+ For Windows client 1\.1\.2\+:

  ```
  C:\Program Files\Amazon\CloudHSM>net.exe start AWSCloudHSMClient
  ```
+ For Windows clients 1\.1\.1 and older:

  ```
  C:\Program Files\Amazon\CloudHSM>start "cloudhsm_client" cloudhsm_client.exe C:\ProgramData\Amazon\CloudHSM\data\cloudhsm_client.cfg
  ```

------

### Start key\_mgmt\_util<a name="key_mgmt_util-start"></a>

After you start the AWS CloudHSM client, use the following command to start key\_mgmt\_util\.

------
#### [ Amazon Linux ]

```
$ /opt/cloudhsm/bin/key_mgmt_util
```

------
#### [ Amazon Linux 2 ]

```
$ /opt/cloudhsm/bin/key_mgmt_util
```

------
#### [ CentOS 7 ]

```
$ /opt/cloudhsm/bin/key_mgmt_util
```

------
#### [ CentOS 8 ]

```
$ /opt/cloudhsm/bin/key_mgmt_util
```

------
#### [ RHEL 7 ]

```
$ /opt/cloudhsm/bin/key_mgmt_util
```

------
#### [ RHEL 8 ]

```
$ /opt/cloudhsm/bin/key_mgmt_util
```

------
#### [ Ubuntu 16\.04 LTS ]

```
$ /opt/cloudhsm/bin/key_mgmt_util
```

------
#### [ Ubuntu 18\.04 LTS ]

```
$ /opt/cloudhsm/bin/key_mgmt_util
```

------
#### [ Windows ]

```
c:\Program Files\Amazon\CloudHSM> .\key_mgmt_util.exe
```

------

The prompt changes to Command: when key\_mgmt\_util is running\.

If the command fails, such as returning a `Daemon socket connection error` message, try [updating your configuration file](troubleshooting-lost-connection.md)\. 

## Basic usage of key\_mgmt\_util<a name="key_mgmt_util-basics"></a>

See the following topics for the basic usage of the key\_mgmt\_util tool\.

**Topics**
+ [Log in to the HSMs](#key_mgmt_util-log-in)
+ [Log out from the HSMs](#key_mgmt_util-log-out)
+ [Stop key\_mgmt\_util](#key_mgmt_util-stop)

### Log in to the HSMs<a name="key_mgmt_util-log-in"></a>

Use the loginHSM command to log in to the HSMs\. The following command logs in as a [crypto user \(CU\)](manage-hsm-users-chsm-cli.md#understanding-users) named `example_user`\. The output indicates a successful login for all three HSMs in the cluster\. 

```
Command:  loginHSM -u CU -s example_user -p <password>
Cfm3LoginHSM returned: 0x00 : HSM Return: SUCCESS

Cluster Error Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

The following shows the syntax for the loginHSM command\.

```
Command:  loginHSM -u <user type> -s <username> -p <password>
```

### Log out from the HSMs<a name="key_mgmt_util-log-out"></a>

Use the logoutHSM command to log out from the HSMs\.

```
Command:  logoutHSM
Cfm3LogoutHSM returned: 0x00 : HSM Return: SUCCESS

Cluster Error Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

### Stop key\_mgmt\_util<a name="key_mgmt_util-stop"></a>

Use the exit command to stop key\_mgmt\_util\.

```
Command:  exit
```