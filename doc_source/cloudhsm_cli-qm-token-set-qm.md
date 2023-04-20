# quorum token\-sign set\-quorum\-value<a name="cloudhsm_cli-qm-token-set-qm"></a>

Use the quorum token\-sign set\-quorum\-value command in CloudHSM CLI to set a new quorum value for a quorum authorized service\.

## User type<a name="quorum-token-set-qm-user-type"></a>

The following users can run this command\.
+ Admin

## Syntax<a name="quorum-token-set-qm-syntax"></a>

```
aws-cloudhsm > help quorum token-sign set-quorum-value 
Set a quorum value

Usage: quorum token-sign set-quorum-value [OPTIONS] --service <SERVICE> --value <VALUE> 

Options:
      --service <SERVICE> 
          Service the token will be used for

          Possible values:
          - user:
            User management service is used for executing quorum authenticated user management operations
          - quorum:
            Quorum management service is used for setting quorum values for any quorum service

      --value <VALUE> 
          Value to set for service

      --approval <APPROVAL> 
          Filepath of signed quorum token file to approve operation
```

## Example<a name="quorum-token-set-qm-examples"></a>

**Example**  
In the following example, this command writes one unsigned token per HSM in your cluster to the file specified by token\. When you are prompted, sign the tokens in the file\.  

```
aws-cloudhsm > quorum token-sign set-quorum-value --service quorum --value 2
{
  "error_code": 0,
  "data": "Set Quorum Value successful"
}
```
You can then run the get\-quorum\-values command to confirm that the quorum value for the quorum management service has been set:  

```
aws-cloudhsm > quorum token-sign get-quorum-values
{
  "error_code": 0,
  "data": {
    "user": 1,
    "quorum": 2
  }
}
```

## Arguments<a name="quorum-token-set-qm-arguments"></a>

***<APPROVAL>***  
The filepath of the signed token file to be approved on the HSM\.

***<SERVICE>***  
Specifies the quorum authorized service for which to generate a token\. This parameter is required\. For more information about service types and names, see [Service names and types that support quorum authentication](quorum-auth-chsm-cli-service-names.md)\.  
**Valid values**  
+ **user**: The user management service\. Service used for executing quorum authorized user management operations\.
+ **quorum**: The quorum management service\. Service used for setting a quorum authorized quorum values for any quorum authorized service\.
+ **registration**: Generates a unsigned token for use in registering a public key for quorum authorization\.
**Required**: Yes

***<VALUE>***  
Specifies The quorum value to be set\. The maximum quorum value is eight \(8\)\.  
**Require**: Yes

## Related topics<a name="quorum-token-set-qm-seealso"></a>
+ [quorum token\-sign list\-quorum\-values](cloudhsm_cli-qm-token-list-qm.md)
+ [Service names and types that support quorum authentication](quorum-auth-chsm-cli-service-names.md)