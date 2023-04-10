# Deregister an MFA public key for admin users when MFA public key is registered<a name="deregister-mfa-cloudhsm-cli"></a>

Follow these steps to deregister an MFA public key for admin users when MFA public key is registered\.

1. Use CloudHSM CLI to log in to the HSM as an admin with MFA enabled\.

1. Use the user change\-mfa token\-sign command to remove MFA for a user\.

   ```
   aws-cloudhsm >user change-mfa token-sign --username <USERNAME> --role admin --deregister --change-quorum
   Enter password:
   Confirm password:
   {
     "error_code": 0,
     "data": {
       "username": "<USERNAME>",
       "role": "admin"
     }
   }
   ```