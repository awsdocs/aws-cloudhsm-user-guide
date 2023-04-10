# Understanding MFA for HSM users<a name="understand-mfa-cloudhsm-cli"></a>

When you log in to a cluster with an MFA enabled HSM user account, you provide the CloudHSM CLI with your password—the first factor, what you know—and CloudHSM CLI provides you with a token and prompts you to have the token signed\.

To provide the second factor—what you have—you sign the token with a private key from a key pair you've already created and associated with the HSM user\. To access the cluster, you provide the signed token to CloudHSM CLI\.

For more information on setting up MFA for a user please see [Set up MFA for CloudHSM CLI](set-up-mfa-for-cloudhsm-cli.md)

## Quorum authentication and MFA<a name="quorum-mfa-cloudhsm-cli"></a>

The cluster uses the same key for quorum authentication and for MFA\. This means a user with MFA enabled is effectively registered for MofN or Quroum access control\. To successfully use MFA and quorum authentication for the same HSM user, consider the following points:
+ If you are using quorum authentication for a user today, you should use the same key pair you created for the quorum user to enable MFA for the user\.
+ If you add the MFA requirement for a non\-MFA user who is not a quorum authentication user, then you register that user as a Quroum \(MofN\) registered user with MFA authentication\.
+ If you remove the MFA requirement or change the password for an MFA user who is also a registered quorum authentication user, you will also remove the user's registration as a quorum \(MofN\) user\.
+ If you remove the MFA requirement or change the password for an MFA user who is also a quorum authentication user, *but you still want that user to participate in quorum authentication*, then you must register that user again as a Quorum \(MofN\) user\.

For more information about quorum authentication, see [Managing M of N](quorum-auth-chsm-cli.md)\.