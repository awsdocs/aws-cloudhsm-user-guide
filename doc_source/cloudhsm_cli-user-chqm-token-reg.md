# user change\-quorum token\-sign register<a name="cloudhsm_cli-user-chqm-token-reg"></a>

Use the user change\-quorum token\-sign register command in CloudHSM CLI to register the token\-sign quorum strategy for an admin user\.

## User type<a name="token-register-user-type"></a>

The following users can run this command\.
+ Admin

## Syntax<a name="token-register-syntax"></a>

```
aws-cloudhsm > help user change-quorum token-sign register
Register a user for quorum authentication with a public key

Usage: user change-quorum token-sign register --public-key <PUBLIC_KEY> --signed-token <SIGNED_TOKEN>

Options:
      --public-key <PUBLIC_KEY>      Filepath to public key PEM file
      --signed-token <SIGNED_TOKEN>  Filepath with token signed by user private key
```

## Example<a name="token-register-examples"></a>

**Example**  
To run this command you will need to be logged in as the user you wish to register quorum token\-sign for\.  

```
aws-cloudhsm > login --username admin1 --role admin 
  Enter password:
{
  "error_code": 0,
  "data": {
    "username": "admin1",
    "role": "admin"
  }
}
```
The user change\-quorum token\-sign register command will register your public key with the HSM\. As a result, it will qualify you as a quorum approver for quorum\-required operations that need a user to obtain quorum signatures to meet the necessary quorum value threshold\.  

```
aws-cloudhsm > user change-quorum token-sign register \
    --public-key /home/mypemfile 
    --signed-token /home/mysignedtoken
{
  "error_code": 0,
  "data": {
    "username": "admin1",
    "role": "admin"
  }
}
```
You can now run the user list command and confirm that quorum token\-sign has been registered for this user\.  

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
        "quorum": [],
        "cluster-coverage": "full"
      },
      {
        "username": "admin1",
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
      }
    ]
  }
}
```

## Arguments<a name="token-register-arguments"></a>

***<PUBLIC\-KEY>***  
Filepath to the public key PEM file\.  
**Required**: Yes

***<SIGNED\-TOKEN>***  
Filepath with token signed by user private key\.  
**Required**: Yes

## Related topics<a name="token-register-seealso"></a>
+ [Using CloudHSM CLI to manage quorum authentication](quorum-auth-chsm-cli.md)
+ [Using quorum authentication for admins: first time setup](quorum-auth-chsm-cli-first-time.md)
+ [Change the quorum minimum value for admins](quorum-auth-chsm-cli-min-value.md)
+ [Service names and types that support quorum authentication](quorum-auth-chsm-cli-service-names.md)