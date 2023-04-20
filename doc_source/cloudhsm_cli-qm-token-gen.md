# quorum token\-sign generate<a name="cloudhsm_cli-qm-token-gen"></a>

Use the quorum token\-sign generate command in CloudHSM CLI to generate a token for a quorum authorized service\.

There is a limit to obtaining one active token per user per service on an HSM cluster for services user and quorum\.

**Note**  
Only admins can generate a service token\.

## <a name="w13aac19c11c17c25b9c11b9"></a>

**Admin Services**: Quorum authentication is used for admin privileged services like creating users, deleting users, changing user passwords, setting quorum values, and deactivating quorum and MFA capabilities\.

Each service type is further broken down into a qualifying service name, which contains a specific set of quorum supported service operations that can be performed\.


****  

| Service name | Service type | Service operations | 
| --- | --- | --- | 
| user | Admin |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/cloudhsm_cli-qm-token-gen.html)  | 
| quorum | Admin |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/cloudhsm_cli-qm-token-gen.html)  | 

## User type<a name="quorum-token-generate-user-type"></a>

The following users can run this command\.
+ Admin
+ Crypto user \(CU\)

## Syntax<a name="quorum-token-generate-syntax"></a>

```
aws-cloudhsm > help quorum token-sign generate 
Generate a token

Usage: quorum token-sign generate --service <SERVICE> --token <TOKEN> 

Options:
      --service <SERVICE>
          Service the token will be used for

          Possible values:
          - user:
            User management service is used for executing quorum authenticated user management operations
          - quorum:
            Quorum management service is used for setting quorum values for any quorum service
          - registration:
            Registration service is used for registering a public key for quorum authentication

      --token <TOKEN> 
          Filepath where the unsigned token file will be written
```

## Example<a name="quorum-token-generate-examples"></a>

This command will write one unsigned token per HSM in your cluster to the file specified by `token`\.

**Example : Write one unsigned token per HSM in your cluster**  

```
aws-cloudhsm > quorum token-sign generate --service user --token /home/tfile
{
  "error_code": 0,
  "data": {
    "filepath": "/home/tfile"
  }
}
```

## Arguments<a name="quorum-token-generate-arguments"></a>

***<SERVICE>***  
Specifies the quorum authorized service for which to generate a token\. This parameter is required\.  
**Valid values**  
+ **user**: The user management service that is used for executing quorum authorized user management operations\.
+ **quorum**: The quorum management service that is used for setting quorum authorized quorum values for any quorum authorized service\.
+ **registration**: Generates an unsigned token for use in registering a public key for quorum authorization\.
**Required**: Yes

***<TOKEN>***  
Filepath where the unsigned token file will be written\.  
**Required**: Yes

## Related topics<a name="quorum-token-generate-seealso"></a>
+ [Service names and types that support quorum authentication](quorum-auth-chsm-cli-service-names.md)