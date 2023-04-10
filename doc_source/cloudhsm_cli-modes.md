# Interactive and single command modes<a name="cloudhsm_cli-modes"></a>

In CloudHSM CLI, you can run commands two different ways: in single command mode and interactive mode\. Interactive mode is designed for users, and single command mode is designed for scripts\.

**Note**  
All commands work in interactive mode and single command mode\.

## Interactive mode<a name="cloudhsm_cli-mode-interactive"></a>

Use the following commands to start CloudHSM CLI interactive mode

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

When using the CLI in Interactive Mode, you can log in to a user account using the login command\.

To list all CloudHSM CLI commands, run the following command:

```
aws-cloudhsm > help
```

To get the syntax for a CloudHSM CLI command, run the following command:

```
aws-cloudhsm > help <command-name> 
```

To get a list of users on the HSMs, enter user list\.

```
aws-cloudhsm > user list
```

To end your CloudHSM CLI session, run the following command:

```
aws-cloudhsm > quit
```

## Single Command mode<a name="cloudhsm_cli-mode-single-command"></a>

If you run CloudHSM CLI using Single Command Mode, you need to set two environment variables to provide credentials: CLOUDHSM\_PIN and CLOUDHSM\_ROLE:

```
$ export CLOUDHSM_ROLE = admin
```

```
$ export CLOUDHSM_PIN = admin_username:admin_password
```

After doing this, you can execute commands using the credentials stored in your environment\.

```
$ cloudhsm-cli user change-password --username alice --role crypto-user
Enter password:
Confirm password:
{
    "error_code": 0,
    "data": {
      "username": "alice",
      "role": "crypto-user"
    }
}
```