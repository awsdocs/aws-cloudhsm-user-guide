# Getting Started with cloudhsm\_mgmt\_util<a name="cloudhsm_mgmt_util"></a>

AWS CloudHSM includes two command line tools with the AWS CloudHSM client software that you installed previously\. One of these tools is cloudhsm\_mgmt\_util\. You use cloudhsm\_mgmt\_util primarily to manage HSM users\.

To use cloudhsm\_mgmt\_util, first connect to your client instance and then install and configure the AWS CloudHSM client software\. Then see the following topics to get started\.


+ [Setup cloudhsm\_mgmt\_util](#cloudhsm_mgmt_util-setup)
+ [Basic Usage of cloudhsm\_mgmt\_util](#cloudhsm_mgmt_util-basics)

## Setup cloudhsm\_mgmt\_util<a name="cloudhsm_mgmt_util-setup"></a>

Complete the following setup before you use cloudhsm\_mgmt\_util\. You need to do these steps the first time you use cloudhsm\_mgmt\_util and when you add or remove HSMs in your cluster\.


+ [Start the AWS CloudHSM Client](#cloudhsm_mgmt_util-start-cloudhsm-client)
+ [Update the cloudhsm\_mgmt\_util Configuration File](#cloudhsm_mgmt_util-update-configuration)

### Start the AWS CloudHSM Client<a name="cloudhsm_mgmt_util-start-cloudhsm-client"></a>

Before you use cloudhsm\_mgmt\_util, start the AWS CloudHSM client\. The client is a daemon that establishes end\-to\-end encrypted communication with the HSMs in your cluster\. When you add or remove HSMs in your cluster, the cluster informs the client of these changes\. The client keeps the current list of HSMs in its configuration file, and cloudhsm\_mgmt\_util can use this file to get an updated list of the cluster's HSM\.

**To start the AWS CloudHSM client**  
Use the following command to start the AWS CloudHSM client\.

```
$ sudo start cloudhsm-client
```

### Update the cloudhsm\_mgmt\_util Configuration File<a name="cloudhsm_mgmt_util-update-configuration"></a>

After you start the AWS CloudHSM client as described in the previous section, you can update the cloudhsm\_mgmt\_util configuration file to include all the HSMs in your cluster\. If you don't do this, you might run into problems because HSM users are not in sync across your cluster's HSMs\.

Use the following command to update the cloudhsm\_mgmt\_util configuration file\.

```
$ sudo /opt/cloudhsm/bin/configure -m
```

## Basic Usage of cloudhsm\_mgmt\_util<a name="cloudhsm_mgmt_util-basics"></a>

See the following topics for the basic usage of the cloudhsm\_mgmt\_util tool\.

**Note**  
The cloudhsm\_mgmt\_util tool doesn't support auto\-completing commands with the **Tab** key\. Don't use the **Tab** key with cloudhsm\_mgmt\_util, because that can make the tool unresponsive\.


+ [Start cloudhsm\_mgmt\_util](#cloudhsm_mgmt_util-start)
+ [Enable End\-to\-End Encryption](#cloudhsm_mgmt_util-enable_e2e)
+ [Log in to the HSMs](#cloudhsm_mgmt_util-log-in)
+ [Log Out from the HSMs](#cloudhsm_mgmt_util-log-out)
+ [Stop cloudhsm\_mgmt\_util](#cloudhsm_mgmt_util-stop)

### Start cloudhsm\_mgmt\_util<a name="cloudhsm_mgmt_util-start"></a>

Use the following command to start cloudhsm\_mgmt\_util\.

```
$ /opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg

Connecting to the server(s), it may take time
depending on the server(s) load, please wait...

Connecting to server '10.0.2.9': hostname '10.0.2.9', port 2225...
Connected to server '10.0.2.9': hostname '10.0.2.9', port 2225.

Connecting to server '10.0.3.11': hostname '10.0.3.11', port 2225...
Connected to server '10.0.3.11': hostname '10.0.3.11', port 2225.

Connecting to server '10.0.1.12': hostname '10.0.1.12', port 2225...
Connected to server '10.0.1.12': hostname '10.0.1.12', port 2225.
```

The prompt changes to aws\-cloudhsm> when cloudhsm\_mgmt\_util is running\.

### Enable End\-to\-End Encryption<a name="cloudhsm_mgmt_util-enable_e2e"></a>

Use the enable\_e2e command to establish end\-to\-end encrypted communication between cloudhsm\_mgmt\_util and the HSMs in your cluster\. You should enable end\-to\-end encryption each time you start cloudhsm\_mgmt\_util\.

```
aws-cloudhsm>enable_e2e
E2E enabled on server 0(10.0.2.9)
E2E enabled on server 1(10.0.3.11)
E2E enabled on server 2(10.0.1.12)
```

### Log in to the HSMs<a name="cloudhsm_mgmt_util-log-in"></a>

Use the loginHSM command to log in to the HSMs\. The following command logs in as the default crypto officer \(CO\) named *admin*\. You set this user's password when you activated the cluster\.

The output shows that the command logged the *admin* user in to all of the HSMs in the cluster\.

```
aws-cloudhsm>loginHSM CO admin <password>
loginHSM success on server 0(10.0.2.9)
loginHSM success on server 1(10.0.3.11)
loginHSM success on server 2(10.0.1.12)
```

The following shows the syntax for the loginHSM command\.

```
aws-cloudhsm>loginHSM <user type> <user name> <password>
```

### Log Out from the HSMs<a name="cloudhsm_mgmt_util-log-out"></a>

Use the logoutHSM command to log out of the HSMs\.

```
aws-cloudhsm>logoutHSM
logoutHSM success on server 0(10.0.2.9)
logoutHSM success on server 1(10.0.3.11)
logoutHSM success on server 2(10.0.1.12)
```

### Stop cloudhsm\_mgmt\_util<a name="cloudhsm_mgmt_util-stop"></a>

Use the quit command to stop cloudhsm\_mgmt\_util\.

```
aws-cloudhsm>quit


disconnecting from servers, please wait...
```