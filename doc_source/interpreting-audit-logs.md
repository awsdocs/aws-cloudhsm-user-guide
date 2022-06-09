# Interpreting HSM audit logs<a name="interpreting-audit-logs"></a>

The events in the HSM audit logs have standard fields\. Some event types have additional fields that capture useful information about the event\. For example, user login and user management events include the user name and user type of the user\. Key management commands include the key handle\.

Several of the fields provide particularly important information\. The `Opcode` identifies the management command that is being recorded\. The `Sequence No` identifies an event in the log stream and indicates the order in which it was recorded\. 

For example, the following example event is the second event \(`Sequence No: 0x1`\) in the log stream for an HSM\. It shows the HSM generating a password encryption key, which is part of its startup routine\.

```
Time: 12/19/17 21:01:17.140812, usecs:1513717277140812
Sequence No : 0x1
Reboot counter : 0xe8
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_GEN_PSWD_ENC_KEY (0x1d)
Session Handle : 0x1004001
Response : 0:HSM Return: SUCCESS
Log type : MINIMAL_LOG_ENTRY (0)
```

The following fields are common to every AWS CloudHSM event in the audit log\. 

**Time**  
The time that the event occurred in the UTC time zone\. The time is displayed as a human\-readable time and Unix time in microseconds\. 

**Reboot counter**  
A 32\-bit persistent ordinal counter that is incremented when the HSM hardware is rebooted\.   
All events in a log stream have the same reboot counter value\. However, the reboot counter might not be unique to a log stream, as it can differ across different HSM instances in the same cluster\.

**Sequence No**  
A 64\-bit ordinal counter that is incremented for each log event\. The first event in each log stream has a sequence number of 0x0\. There should be no gaps in the `Sequence No` values\. The sequence number is unique only within a log stream\.

**Command type**  
A hexadecimal value that represents the category of the command\. Commands in the AWS CloudHSM log streams have a command type of `CN_MGMT_CMD` \(0x0\) or `CN_CERT_AUTH_CMD` \(0x9\)\.

**Opcode**  
Identifies the management command that was executed\. For a list of `Opcode` values in the AWS CloudHSM audit logs, see [HSM audit log reference](cloudhsm-audit-log-reference.md)\.

**Session handle**  
Identifies the session in which the command was run and the event was logged\.

**Response**  
Records the response to the management command\. You can search the `Response` field for `SUCCESS` and `ERROR` values\.

**Log type**  
Indicates the log type of the AWS CloudHSM log that recorded the command\.  
+ `MINIMAL_LOG_ENTRY (0)`
+ `MGMT_KEY_DETAILS_LOG (1)`
+ `MGMT_USER_DETAILS_LOG (2)`
+ `GENERIC_LOG`

## Examples of audit log events<a name="example-audit-log"></a>

The events in a log stream record the history of the HSM from its creation to deletion\. You can use the log to review the lifecycle of your HSMs and gain insight into its operation\. When you interpret the events, note the `Opcode`, which indicates the management command or action, and the `Sequence No`, which indicates the order of events\. 

**Topics**
+ [Example: Initialize the first HSM in a cluster](#example-audit-log-first-hsm)
+ [Login and logout events](#example-audit-log-login-logout)
+ [Example: Create and delete users](#example-audit-log-first-hsm)
+ [Example: Create and delete a key pair](#example-audit-log-manage-keys)
+ [Example: Generate and synchronize a key](#audit-log-example-gen-key)
+ [Example: Export a key](#audit-log-example-export-key)
+ [Example: Import a key](#audit-log-example-import-key)
+ [Example: Share and unshare a key](#audit-log-example-share-unshare-key)

### Example: Initialize the first HSM in a cluster<a name="example-audit-log-first-hsm"></a>

The audit log stream for the first HSM in each cluster differs significantly from the log streams of other HSMs in the cluster\. The audit log for the first HSM in each cluster records its creation and initialization\. The logs of additional HSMs in the cluster, which are generated from backups, begin with a login event\.

**Important**  
The following initialization entries will not appear in the CloudWatch logs of clusters initialized before the release of the CloudHSM audit logging feature \(August 30, 2018\)\. For more information, see [Document History](document-history.md)\.

The following example events appear in the log stream for the first HSM in a cluster\. The first event in the log — the one with `Sequence No 0x0` — represents the command to initialize the HSM \(`CN_INIT_TOKEN`\)\. The response indicates that the command was successful \(`Response : 0: HSM Return: SUCCESS`\)\.

```
Time: 12/19/17 21:01:16.962174, usecs:1513717276962174
Sequence No : 0x0
Reboot counter : 0xe8
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_INIT_TOKEN (0x1)
Session Handle : 0x1004001
Response : 0:HSM Return: SUCCESS
Log type : MINIMAL_LOG_ENTRY (0)
```

The second event in this example log stream \(`Sequence No 0x1`\) records the command to create the password encryption key that the HSM uses \(`CN_GEN_PSWD_ENC_KEY`\)\. 

This is a typical startup sequence for the first HSM in each cluster\. Because subsequent HSMs in the same cluster are clones of the first one, they use the same password encryption key\.

```
Time: 12/19/17 21:01:17.140812, usecs:1513717277140812
Sequence No : 0x1
Reboot counter : 0xe8
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_GEN_PSWD_ENC_KEY (0x1d)
Session Handle : 0x1004001
Response : 0:HSM Return: SUCCESS
Log type : MINIMAL_LOG_ENTRY (0)
```

The third event in this example log stream \(`Sequence No 0x2`\) is the creation of the [appliance user \(AU\)](manage-hsm-users.md#appliance-user), which is the AWS CloudHSM service\. Events that involve HSM users include extra fields for the user name and user type\. 

```
Time: 12/19/17 21:01:17.174902, usecs:1513717277174902
Sequence No : 0x2
Reboot counter : 0xe8
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_CREATE_APPLIANCE_USER (0xfc)
Session Handle : 0x1004001
Response : 0:HSM Return: SUCCESS
Log type : MGMT_USER_DETAILS_LOG (2)
User Name : app_user
User Type : CN_APPLIANCE_USER (5)
```

The fourth event in this example log stream \(`Sequence No 0x3`\) records the `CN_INIT_DONE` event, which completes the initialization of the HSM\. 

```
Time: 12/19/17 21:01:17.298914, usecs:1513717277298914
Sequence No : 0x3
Reboot counter : 0xe8
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_INIT_DONE (0x95)
Session Handle : 0x1004001
Response : 0:HSM Return: SUCCESS
Log type : MINIMAL_LOG_ENTRY (0)
```

You can follow the remaining events in the startup sequence\. These events might include several login and logout events, and the generation of the key encryption key \(KEK\)\. The following event records the command that changes the password of the [precrypto officer \(PRECO\)](manage-hsm-users.md#preco)\. This command activates the cluster\.

```
Time: 12/13/17 23:04:33.846554, usecs:1513206273846554
Sequence No: 0x1d
Reboot counter: 0xe8
Command Type(hex): CN_MGMT_CMD (0x0)
Opcode: CN_CHANGE_PSWD (0x9)
Session Handle: 0x2010003
Response: 0:HSM Return: SUCCESS
Log type: MGMT_USER_DETAILS_LOG (2)
User Name: admin
User Type: CN_CRYPTO_PRE_OFFICER (6)
```

### Login and logout events<a name="example-audit-log-login-logout"></a>

When interpreting your audit log, note events that record users logging and in and out of the HSM\. These events help you to determine which user is responsible for management commands that appear in sequence between the login and logout commands\.

For example, this log entry records a login by a crypto officer named `admin`\. The sequence number, `0x0`, indicates that this is the first event in this log stream\. 

When a user logs into an HSM, the other HSMs in the cluster also record a login event for the user\. You can find the corresponding login events in the log streams of other HSMs in the cluster shortly after the initial login event\. 

```
Time: 01/16/18 01:48:49.824999, usecs:1516067329824999
Sequence No : 0x0
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_LOGIN (0xd)
Session Handle : 0x7014006
Response : 0:HSM Return: SUCCESS
Log type : MGMT_USER_DETAILS_LOG (2)
User Name : admin
User Type : CN_CRYPTO_OFFICER (2)
```

The following example event records the `admin` crypto officer logging out\. The sequence number, `0x2`, indicates that this is the third event in the log stream\. 

If the logged in user closes the session without logging out, the log stream includes an `CN_APP_FINALIZE` or close session event \(`CN_SESSION_CLOSE`\), instead of a `CN_LOGOUT` event\. Unlike the login event, this logout event typically is recorded only by the HSM that executes the command\.

```
Time: 01/16/18 01:49:55.993404, usecs:1516067395993404
Sequence No : 0x2
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_LOGOUT (0xe)
Session Handle : 0x7014000
Response : 0:HSM Return: SUCCESS
Log type : MGMT_USER_DETAILS_LOG (2)
User Name : admin
User Type : CN_CRYPTO_OFFICER (2)
```

If a login attempt fails because the user name is invalid, the HSM records a `CN_LOGIN` event with the user name and type provided in the login command\. The response displays error message 157, which explains that the user name does not exist\.

```
Time: 01/24/18 17:41:39.037255, usecs:1516815699037255
Sequence No : 0x4
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_LOGIN (0xd)
Session Handle : 0xc008002
Response : 157:HSM Error: user isn't initialized or user with this name doesn't exist
Log type : MGMT_USER_DETAILS_LOG (2)
User Name : ExampleUser
User Type : CN_CRYPTO_USER (1)
```

If a login attempt fails because the password is invalid, the HSM records a `CN_LOGIN` event with the user name and type provided in the login command\. The response displays the error message with the `RET_USER_LOGIN_FAILURE` error code\.

```
Time: 01/24/18 17:44:25.013218, usecs:1516815865013218
Sequence No : 0x5
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_LOGIN (0xd)
Session Handle : 0xc008002
Response : 163:HSM Error: RET_USER_LOGIN_FAILURE
Log type : MGMT_USER_DETAILS_LOG (2)
User Name : testuser
User Type : CN_CRYPTO_USER (1)
```

### Example: Create and delete users<a name="example-audit-log-first-hsm"></a>

This example shows the log events that are recorded when a crypto officer \(CO\) creates and deletes users\. 

The first event records a CO, `admin`, logging into the HSM\. The sequence number of `0x0` indicates that this is the first event in the log stream\. The name and type of the user who logged in are included in the event\.

```
Time: 01/16/18 01:48:49.824999, usecs:1516067329824999
Sequence No : 0x0
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_LOGIN (0xd)
Session Handle : 0x7014006
Response : 0:HSM Return: SUCCESS
Log type : MGMT_USER_DETAILS_LOG (2)
User Name : admin
User Type : CN_CRYPTO_OFFICER (2)
```

The next event in the log stream \(sequence `0x1`\) records the CO creating a new crypto user \(CU\)\. The name and type of the new user are included in the event\. 

```
Time: 01/16/18 01:49:39.437708, usecs:1516067379437708
Sequence No : 0x1
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_CREATE_USER (0x3)
Session Handle : 0x7014006
Response : 0:HSM Return: SUCCESS
Log type : MGMT_USER_DETAILS_LOG (2)
User Name : bob
User Type : CN_CRYPTO_USER (1)
```

Then, the CO creates another crypto officer, `alice`\. The sequence number indicates that this action followed the previous one with no intervening actions\.

```
Time: 01/16/18 01:49:55.993404, usecs:1516067395993404
Sequence No : 0x2
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_CREATE_CO (0x4)
Session Handle : 0x7014007
Response : 0:HSM Return: SUCCESS
Log type : MGMT_USER_DETAILS_LOG (2)
User Name : alice
User Type : CN_CRYPTO_OFFICER (2)
```

Later, the CO named `admin` logs in and deletes the crypto officer named `alice`\. The HSM records a `CN_DELETE_USER` event\. The name and type of the deleted user are included in the event\.

```
Time: 01/23/18 19:58:23.451420, usecs:1516737503451420
Sequence No : 0xb
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_DELETE_USER (0xa1)
Session Handle : 0x7014007
Response : 0:HSM Return: SUCCESS
Log type : MGMT_USER_DETAILS_LOG (2)
User Name : alice
User Type : CN_CRYPTO_OFFICER (2)
```

### Example: Create and delete a key pair<a name="example-audit-log-manage-keys"></a>

This example shows the events that are recorded in an HSM audit log when you create and delete a key pair\. 

The following event records the crypto user \(CU\) named `crypto_user` logging in to the HSM\. 

```
Time: 12/13/17 23:09:04.648952, usecs:1513206544648952
Sequence No: 0x28
Reboot counter: 0xe8
Command Type(hex): CN_MGMT_CMD (0x0)
Opcode: CN_LOGIN (0xd)
Session Handle: 0x2014005
Response: 0:HSM Return: SUCCESS
Log type: MGMT_USER_DETAILS_LOG (2)
User Name: crypto_user
User Type: CN_CRYPTO_USER (1)
```

Next, the CU generates a key pair \(`CN_GENERATE_KEY_PAIR`\)\. The private key has key handle `131079`\. The public key has key handle `131078`\. 

```
Time: 12/13/17 23:09:04.761594, usecs:1513206544761594
Sequence No: 0x29
Reboot counter: 0xe8
Command Type(hex): CN_MGMT_CMD (0x0)
Opcode: CN_GENERATE_KEY_PAIR (0x19)
Session Handle: 0x2014004
Response: 0:HSM Return: SUCCESS
Log type: MGMT_KEY_DETAILS_LOG (1)
Priv/Secret Key Handle: 131079
Public Key Handle: 131078
```

The CU immediately deletes the key pair\. A CN\_DESTROY\_OBJECT event records the deletion of the public key \(131078\)\. 

```
Time: 12/13/17 23:09:04.813977, usecs:1513206544813977
Sequence No: 0x2a
Reboot counter: 0xe8
Command Type(hex): CN_MGMT_CMD (0x0)
Opcode: CN_DESTROY_OBJECT (0x11)
Session Handle: 0x2014004
Response: 0:HSM Return: SUCCESS
Log type: MGMT_KEY_DETAILS_LOG (1)
Priv/Secret Key Handle: 131078
Public Key Handle: 0
```

Then, a second `CN_DESTROY_OBJECT` event records the deletion of the private key \(`131079`\)\.

```
Time: 12/13/17 23:09:04.815530, usecs:1513206544815530
Sequence No: 0x2b
Reboot counter: 0xe8
Command Type(hex): CN_MGMT_CMD (0x0)
Opcode: CN_DESTROY_OBJECT (0x11)
Session Handle: 0x2014004
Response: 0:HSM Return: SUCCESS
Log type: MGMT_KEY_DETAILS_LOG (1)
Priv/Secret Key Handle: 131079
Public Key Handle: 0
```

Finally, the CU logs out\. 

```
Time: 12/13/17 23:09:04.817222, usecs:1513206544817222
Sequence No: 0x2c
Reboot counter: 0xe8
Command Type(hex): CN_MGMT_CMD (0x0)
Opcode: CN_LOGOUT (0xe)
Session Handle: 0x2014004
Response: 0:HSM Return: SUCCESS
Log type: MGMT_USER_DETAILS_LOG (2)
User Name: crypto_user
User Type: CN_CRYPTO_USER (1)
```

### Example: Generate and synchronize a key<a name="audit-log-example-gen-key"></a>

This example shows the effect of creating a key in a cluster with multiple HSMs\. The key is generated on one HSM, extracted from the HSM as a masked object, and inserted in the other HSMs as a masked object\.

**Note**  
The client tools might fail to synchronize the key\. Or the command might include the min\_srv parameter, which synchronizes the key only to the specified number of HSMs\. In either case, the AWS CloudHSM service synchronizes the key to the other HSMs in the cluster\. Because the HSMs record only client\-side management commands in their logs, the server\-side synchronization is not recorded in the HSM log\.

First consider the log stream of the HSM that receives and executes the commands\. The log stream is named for HSM ID, `hsm-abcde123456`, but the HSM ID does not appear in the log events\. 

First, the `testuser` crypto user \(CU\) logs in to the `hsm-abcde123456` HSM\.

```
Time: 01/24/18 00:39:23.172777, usecs:1516754363172777
Sequence No : 0x0
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_LOGIN (0xd)
Session Handle : 0xc008002
Response : 0:HSM Return: SUCCESS
Log type : MGMT_USER_DETAILS_LOG (2)
User Name : testuser
User Type : CN_CRYPTO_USER (1)
```

The CU runs an [exSymKey](key_mgmt_util-genSymKey.md) command to generate a symmetric key\. The `hsm-abcde123456` HSM generates a symmetric key with a key handle of `262152`\. The HSM records a `CN_GENERATE_KEY` event in its log\. 

```
Time: 01/24/18 00:39:30.328334, usecs:1516754370328334
Sequence No : 0x1
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_GENERATE_KEY (0x17)
Session Handle : 0xc008004
Response : 0:HSM Return: SUCCESS
Log type : MGMT_KEY_DETAILS_LOG (1)
Priv/Secret Key Handle : 262152
Public Key Handle : 0
```

The next event in the log stream for `hsm-abcde123456` records the first step in the key synchronization process\. The new key \(key handle `262152`\) is extracted from the HSM as a masked object\. 

```
Time: 01/24/18 00:39:30.330956, usecs:1516754370330956
Sequence No : 0x2
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_EXTRACT_MASKED_OBJECT_USER (0xf0)
Session Handle : 0xc008004
Response : 0:HSM Return: SUCCESS
Log type : MGMT_KEY_DETAILS_LOG (1)
Priv/Secret Key Handle : 262152
Public Key Handle : 0
```

Now consider the log stream for HSM `hsm-zyxwv987654`, another HSM in the same cluster\. This log stream also includes a login event for the `testuser` CU\. The time value shows that occurs shortly after the user logs in to the `hsm-abcde123456` HSM\.

```
Time: 01/24/18 00:39:23.199740, usecs:1516754363199740
Sequence No : 0xd
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_LOGIN (0xd)
Session Handle : 0x7004004
Response : 0:HSM Return: SUCCESS
Log type : MGMT_USER_DETAILS_LOG (2)
User Name : testuser
User Type : CN_CRYPTO_USER (1)
```

This log stream for this HSM does not have a `CN_GENERATE_KEY` event\. But it does have an event that records synchronization of the key to this HSM\. The `CN_INSERT_MASKED_OBJECT_USER` event records the receipt of key `262152` as a masked object\. Now key `262152` exists on both HSMs in the cluster\.

```
Time: 01/24/18 00:39:30.408950, usecs:1516754370408950
Sequence No : 0xe
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_INSERT_MASKED_OBJECT_USER (0xf1)
Session Handle : 0x7004003
Response : 0:HSM Return: SUCCESS
Log type : MGMT_KEY_DETAILS_LOG (1)
Priv/Secret Key Handle : 262152
Public Key Handle : 0
```

When the CU user logs out, this `CN_LOGOUT` event appears only in the log stream of the HSM that received the commands\. 

### Example: Export a key<a name="audit-log-example-export-key"></a>

This example shows the audit log events that are recorded when a crypto user \(CU\) exports keys from a cluster with multiple HSMs\. 

The following event records the CU \(`testuser`\) logging into [key\_mgmt\_util](key_mgmt_util.md)\. 

```
Time: 01/24/18 19:42:22.695884, usecs:1516822942695884
Sequence No : 0x26
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_LOGIN (0xd)
Session Handle : 0x7004004
Response : 0:HSM Return: SUCCESS
Log type : MGMT_USER_DETAILS_LOG (2)
User Name : testuser
User Type : CN_CRYPTO_USER (1)
```

The CU runs an [exSymKey](key_mgmt_util-exSymKey.md) command to export key `7`, a 256\-bit AES key\. The command uses key `6`, a 256\-bit AES key on the HSMs, as the wrapping key\. 

The HSM that receives the command records a `CN_WRAP_KEY` event for key `7`, the key that is being exported\. 

```
Time: 01/24/18 19:51:12.860123, usecs:1516823472860123
Sequence No : 0x27
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_WRAP_KEY (0x1a)
Session Handle : 0x7004003
Response : 0:HSM Return: SUCCESS
Log type : MGMT_KEY_DETAILS_LOG (1)
Priv/Secret Key Handle : 7
Public Key Handle : 0
```

Then, the HSM records a `CN_NIST_AES_WRAP` event for the wrapping key, key `6`\. The key is wrapped and then immediately unwrapped, but the HSM records only one event\.

```
Time: 01/24/18 19:51:12.905257, usecs:1516823472905257
Sequence No : 0x28
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_NIST_AES_WRAP (0x1e)
Session Handle : 0x7004003
Response : 0:HSM Return: SUCCESS
Log type : MGMT_KEY_DETAILS_LOG (1)
Priv/Secret Key Handle : 6
Public Key Handle : 0
```

The exSymKey command writes the exported key to a file but does not change the key on the HSM\. Consequently, there are no corresponding events in the logs of other HSMs in the cluster\. 

### Example: Import a key<a name="audit-log-example-import-key"></a>

This example shows the audit log events that are recorded when you import keys into the HSMs in a cluster\. In this example, the crypto user \(CU\) uses the [imSymKey](key_mgmt_util-imSymKey.md) command to import an AES key into the HSMs\. The command uses key `6` as the wrapping key\. 

The HSM that receives the commands first records a `CN_NIST_AES_WRAP` event for key `6`, the wrapping key\. 

```
Time: 01/24/18 19:58:23.170518, usecs:1516823903170518
Sequence No : 0x29
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_NIST_AES_WRAP (0x1e)
Session Handle : 0x7004003
Response : 0:HSM Return: SUCCESS
Log type : MGMT_KEY_DETAILS_LOG (1)
Priv/Secret Key Handle : 6
Public Key Handle : 0
```

Then, the HSM records a `CN_UNWRAP_KEY` event that represents the import operation\. The imported key is assigned a key handle of `11`\.

```
Time: 01/24/18 19:58:23.200711, usecs:1516823903200711
Sequence No : 0x2a
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_UNWRAP_KEY (0x1b)
Session Handle : 0x7004003
Response : 0:HSM Return: SUCCESS
Log type : MGMT_KEY_DETAILS_LOG (1)
Priv/Secret Key Handle : 11
Public Key Handle : 0
```

When a new key is generated or imported, the client tools automatically attempt to synchronize the new key to other HSMs in the cluster\. In this case, the HSM records a `CN_EXTRACT_MASKED_OBJECT_USER` event when key `11` is extracted from the HSM as a masked object\.

```
Time: 01/24/18 19:58:23.203350, usecs:1516823903203350
Sequence No : 0x2b
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_EXTRACT_MASKED_OBJECT_USER (0xf0)
Session Handle : 0x7004003
Response : 0:HSM Return: SUCCESS
Log type : MGMT_KEY_DETAILS_LOG (1)
Priv/Secret Key Handle : 11
Public Key Handle : 0
```

The log streams of other HSMs in the cluster reflect the arrival of the newly imported key\. 

For example, this event was recorded in the log stream of a different HSM in the same cluster\. This `CN_INSERT_MASKED_OBJECT_USER` event records the arrival of a masked object that represents key `11`\.

```
Time: 01/24/18 19:58:23.286793, usecs:1516823903286793
Sequence No : 0xb
Reboot counter : 0x107
Command Type(hex) : CN_MGMT_CMD (0x0)
Opcode : CN_INSERT_MASKED_OBJECT_USER (0xf1)
Session Handle : 0xc008004
Response : 0:HSM Return: SUCCESS
Log type : MGMT_KEY_DETAILS_LOG (1)
Priv/Secret Key Handle : 11
Public Key Handle : 0
```

### Example: Share and unshare a key<a name="audit-log-example-share-unshare-key"></a>

This example shows the audit log event that is recorded when a crypto user \(CU\) shares or unshares ECC private key with other crypto users\. The CU uses the [shareKey](cloudhsm_mgmt_util-shareKey.md) command and provides the key handle, the user ID, and the value `1` to share or value `0` to unshare the key\. 

In the following example, the HSM that receives the command, records a `CM_SHARE_OBJECT` event that represents the share operation\.

```
Time: 02/08/19 19:35:39.480168, usecs:1549654539480168
Sequence No	: 0x3f
Reboot counter	: 0x38
Command Type(hex)	: CN_MGMT_CMD (0x0)
Opcode	: CN_SHARE_OBJECT (0x12)
Session Handle	: 0x3014007
Response	: 0:HSM Return: SUCCESS
Log type	: UNKNOWN_LOG_TYPE (5)
```