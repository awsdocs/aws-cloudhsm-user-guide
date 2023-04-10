# Using quorum authentication for admins: first time setup<a name="quorum-auth-chsm-cli-first-time"></a>

The following topics describe the steps that you must complete to configure your hardware security module \(HSM\) so that [admins](manage-hsm-users-chsm-cli.md#admin) can use quorum authentication\. You need to do these steps only once when you first configure quorum authentication for admins\. After you complete these steps, see [Using quorum authentication for admins](quorum-auth-chsm-cli-admin.md)\.

**Topics**
+ [Prerequisites](#quorum-admin-prerequisites)
+ [Create and register a key for signing](#quorum-admin-create-and-register-key)
+ [Set the quorum minimum value on the HSM](#quorum-admin-set-quorum-minimum-value-chsm-cli)

## Prerequisites<a name="quorum-admin-prerequisites"></a>

To understand this example, you should be familiar with [CloudHSM CLI](cloudhsm_cli.md)\. In this example, the AWS CloudHSM cluster has two HSMs, each with the same admins, as shown in the following output from the user list command\. For more information about creating users, see [Using CloudHSM CLI](manage-hsm-users-chsm-cli.md)\.

```
aws-cloudhsm>user list
{
  "error_code": 0,
  "data": {
    "users": [
      {
        "username": "admin",
        "role": "admin",
        "locked": "false",
        "mfa": [],
        "quorum": [],
        "cluster-coverage": "full"
      },
      {
        "username": "admin2",
        "role": "admin",
        "locked": "false",
        "mfa": [],
        "quorum": [],
        "cluster-coverage": "full"
      },
      {
        "username": "admin3",
        "role": "admin",
        "locked": "false",
        "mfa": [],
        "quorum": [],
        "cluster-coverage": "full"
      },
      {
        "username": "admin4",
        "role": "admin",
        "locked": "false",
        "mfa": [],
        "quorum": [],
        "cluster-coverage": "full"
      },
      {
        "username": "app_user",
        "role": "internal(APPLIANCE_USER)",
        "locked": "false",
        "mfa": [],
        "quorum": [],
        "cluster-coverage": "full"
      }
    ]
  }
}
```

## Create and register a key for signing<a name="quorum-admin-create-and-register-key"></a>

To use quorum authentication, each admin must complete *all* of the following steps: 

**Topics**
+ [Create an RSA key pair](#mofn-key-pair-create-chsm-cli)
+ [Create and sign a registration token](#mofn-registration-token-chsm-cli)
+ [Register the public key with the HSM](#mofn-register-key-chsm-cli)

### Create an RSA key pair<a name="mofn-key-pair-create-chsm-cli"></a>

There are many different ways to create and protect a key pair\. The following examples show how to do it with [OpenSSL](https://www.openssl.org/)\.

**Example – Create a private key with OpenSSL**  
The following example demonstrates how to use OpenSSL to create a 2048\-bit RSA key that is protected by a pass phrase\. To use this example, replace *<admin\.key>* with the name of the file where you want to store the key\.  

```
$ openssl genrsa -out <admin.key> -aes256 2048
Generating RSA private key, 2048 bit long modulus
.....................................+++
.+++
e is 65537 (0x10001)
Enter pass phrase for admin.key:
Verifying - Enter pass phrase for admin.key:
```

Next, generate the public key using the private key that you just created\.

**Example – Create a public key with OpenSSL**  
The following example demonstrates how to use OpenSSL to create a public key from the private key you just created\.  

```
$ openssl rsa -in admin.key -outform PEM -pubout -out admin1.pub
Enter pass phrase for admin.key:
writing RSA key
```

### Create and sign a registration token<a name="mofn-registration-token-chsm-cli"></a>

You create a token and sign it with the private key you just generated in the previous step\.

**Example – Create a registration token**  

1. Use the following command to start the CloudHSM CLI:

------
#### [ Linux ]

   ```
   $ /opt/cloudhsm/bin/cloudhsm-cli interactive
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM\bin\> .\cloudhsm-cli.exe interactive
   ```

------

1. Create a registration token by running the [quorum token\-sign generate](cloudhsm_cli-qm-token-gen.md) command:

   ```
   aws-cloudhsm > quorum token-sign generate --service registration --token /path/tokenfile
   {
     "error_code": 0,
     "data": {
       "path": "/path/tokenfile"
     }
   }
   ```

1. The [quorum token\-sign generate](cloudhsm_cli-qm-token-gen.md) command generates a registration token at the specified file path\. Inspect the token file:

   ```
   $ cat /path/tokenfile{
     "version": "2.0",
     "tokens": [
       {
         "approval_data": <approval data in base64 encoding>,
         "unsigned": <unsigned token in base64 encoding>,
         "signed": ""
       }
     ]
   }
   ```

   The token file consists of the following:
   + **approval\_data**: A base64 encoded randomized data token whose raw data doesn’t exceed the maximum of 245 bytes\.
   + **unsigned**: A base64 encoded and SHA256 hashed token of the approval\_data\.
   + **signed**: A base64 encoded signed token \(signature\) of the unsigned token, using the RSA 2048\-bit private key previously generated with OpenSSL\.

   You sign the unsigned token with the private key to demonstrate that you have access to the private key\. You will need the registration token file fully populated with a signature and the public key to register the admin as a quorum user with the AWS CloudHSM cluster\.

**Example – Sign the unsigned registration token**  

1. Decode the base64 encoded unsigned token and place it into a binary file:

   ```
   $ echo -n '6BMUj6mUjjko6ZLCEdzGlWpR5sILhFJfqhW1ej3Oq1g=' | base64 -d > admin.bin
   ```

1. Use OpenSSL and the private key to sign the now binary unsigned registration token and create a binary signature file:

   ```
   $ openssl pkeyutl -sign \
   -inkey admin.key \
   -pkeyopt digest:sha256 \
   -keyform PEM \
   -in admin.bin \
   -out admin.sig.bin
   ```

1. Encode the binary signature into base64:

   ```
   $ base64 -w0 admin.sig.bin > admin.sig.b64
   ```

1. Copy and paste the base64 encoded signature into the token file:

   ```
   {
     "version": "2.0",
     "tokens": [
       {
         "approval_data": <approval data in base64 encoding>,
         "unsigned": <unsigned token in base64 encoding>,
         "signed": <signed token in base64 encoding>
       }
     ]
   }
   ```

### Register the public key with the HSM<a name="mofn-register-key-chsm-cli"></a>

After creating a key, the admin must register the public key with the AWS CloudHSM cluster\.

**To register a public key with the HSM**

1. Use the following command to start CloudHSM CLI:

------
#### [ Linux ]

   ```
   $ /opt/cloudhsm/bin/cloudhsm-cli interactive
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM\bin\> .\cloudhsm-cli.exe interactive
   ```

------

1. Using CloudHSM CLI, log in as an admin\.

   ```
   aws-cloudhsm > login --username admin --role admin
   Enter password:
   {
     "error_code": 0,
     "data": {
       "username": "admin",
       "role": "admin"
     }
   }
   ```

1. Use the [user change\-quorum token\-sign activate](cloudhsm_cli-user-chqm-token-reg.md) command to register the public key\. For more information, see the following example or use the help user change\-quorum token\-sign activate command\.

**Example – Register a public key with AWS CloudHSM cluster**  
The following example shows how to use the user change\-quorum token\-sign activate command in CloudHSM CLI to register an admin' public key with the HSM\. To use this command, the admin must be logged in to the HSM\. Replace these values with your own:  

```
aws-cloudhsm > user change-quorum token-sign activate --public-key </path/admin.pub> --signed-token </path/tokenfile>
     {
  "error_code": 0,
  "data": {
    "username": "admin",
    "role": "admin"
  }
}
```
**/path/admin\.pub**: The filepath to the public key PEM file  
**Required**: Yes  
**/path/tokenfile**: The filepath with token signed by user private key  
**Required**: Yes
After all admins register their public keys, the output from the user list command shows this in the quorum field, stating the enabled quorum strategy in use, as shown below:  

```
aws-cloudhsm > user list
     {
  "error_code": 0,
  "data": {
    "users": [
      {
        "username": "admin",
        "role": "admin",
        "locked": "false",
        "mfa": [],
        "quorum": [
          {
            "strategy": "token-sign",
            "status": "enabled"
          }
        ],
        "cluster-coverage": "full"
      },
      {
        "username": "admin2",
        "role": "admin",
        "locked": "false",
        "mfa": [],
        "quorum": [
          {
            "strategy": "token-sign",
            "status": "enabled"
          }
        ],
        "cluster-coverage": "full"
      },
      {
        "username": "admin3",
        "role": "admin",
        "locked": "false",
        "mfa": [],
        "quorum": [
          {
            "strategy": "token-sign",
            "status": "enabled"
          }
        ],
        "cluster-coverage": "full"
      },
      {
        "username": "admin4",
        "role": "admin",
        "locked": "false",
        "mfa": [],
        "quorum": [
          {
            "strategy": "token-sign",
            "status": "enabled"
          }
        ],
        "cluster-coverage": "full"
      },
      {
        "username": "app_user",
        "role": "internal(APPLIANCE_USER)",
        "locked": "false",
        "mfa": [],
        "quorum": [],
        "cluster-coverage": "full"
      }
    ]
  }
}
```

## Set the quorum minimum value on the HSM<a name="quorum-admin-set-quorum-minimum-value-chsm-cli"></a>

To use quorum authentication, an admin must log in to the HSM and then set the *quorum minimum value*\. This is the minimum number of admin approvals that are required to perform HSM user management operations\. Any admin on the HSM can set the quorum minimum value, including admins who have not registered a key for signing\. You can change the quorum minimum value at any time\. for more information, see [Change the minimum value](quorum-auth-chsm-cli-min-value.md)\.

**To set the quorum minimum value on the HSM**

1. Use the following command to start CloudHSM CLI:

------
#### [ Linux ]

   ```
   $ /opt/cloudhsm/bin/cloudhsm-cli interactive
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM\bin\> .\cloudhsm-cli.exe interactive
   ```

------

1. Using CloudHSM CLI, log in as an admin\.

   ```
   aws-cloudhsm > login --username admin --role admin
   Enter password:
   {
     "error_code": 0,
     "data": {
       "username": "admin",
       "role": "admin"
     }
   }
   ```

1. Use the [user change\-quorum token\-sign register](cloudhsm_cli-user-chqm-token-reg.md) command to set the quorum minimum value\. For more information, see the following example or use the help quorum token\-sign set\-quorum\-value command\.

**Example – Set the quorum minimum value on the HSM**  
This example uses a quorum minimum value of two \(2\)\. You can choose any value from two \(2\) to eight \(8\), up to the total number of admins on the HSM\. In this example, the HSM has four \(4\) admins, so the maximum possible value is four \(4\)\.  
To use the following example command, replace the final number \(*<2>*\) with the preferred quorum minimum value\.  

```
aws-cloudhsm > quorum token-sign set-quorum-value --service user --value <2>
{
  "error_code": 0,
  "data": "Set quorum value successful"
}
```
In this example, the service identifies the HSM service whose quorum minimum value you are setting\. The [ quorum token\-sign generate](cloudhsm_cli-qm-token-gen.md) command lists the HSM service types, names, and descriptions that are included in the service\.   
**Admin Services**: Quorum authentication is used for admin privileged services like creating users, deleting users, changing user passwords, setting quorum values, and deactivating quorum and MFA capabilities\.  
Each service type is further broken down into a qualifying service name, which contains a specific set of quorum supported service operations that can be performed\.  


****  

| Service name | Service type | Service operations | 
| --- | --- | --- | 
| user | Admin |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/quorum-auth-chsm-cli-first-time.html)  | 
| quorum | Admin |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/quorum-auth-chsm-cli-first-time.html)  | 
To get the quorum minimum value for a service, use the quorum token\-sign list\-quorum\-values command:  

```
aws-cloudhsm > quorum token-sign list-quorum-values
{
  "error_code": 0,
  "data": {
    "user": 2,
    "quorum": 1
  }
}
```

The output from the preceding quorum token\-sign list\-quorum\-values command shows that the quorum minimum value for HSM user service, responsible for user management operations, is now two \(2\)\. After you complete these steps, see [Using M of N](quorum-auth-chsm-cli-admin.md)\.