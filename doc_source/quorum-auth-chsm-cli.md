# Using CloudHSM CLI to manage quorum authentication \(M of N access control\)<a name="quorum-auth-chsm-cli"></a>

The HSMs in your AWS CloudHSM cluster support quorum authentication, which is also known as M of N access control\. With quorum authentication, no single user on the HSM can do quorum\-controlled operations on the HSM\. Instead, a minimum number of HSM users \(at least 2\) must cooperate to do these operations\. With quorum authentication, you can add an extra layer of protection by requiring approvals from more than one HSM user\.

Quorum authentication can control the following operations:
+ HSM user management by [admin](manage-hsm-users-chsm-cli.md#admin) – Creating and deleting HSM users, and changing a different HSM user's password\. For more information, see [Using quorum authentication for admins](quorum-auth-chsm-cli-admin.md)\.

The following topics provide more information about quorum authentication in AWS CloudHSM\.

**Topics**
+ [Overview of quorum authentication with token\-sign strategy](#quorum-auth-chsm-cli-overview)
+ [Additional details about quorum authentication](#quorum-authentication-chsm-cli-details)
+ [Service names and types that support quorum authentication](quorum-auth-chsm-cli-service-names.md)
+ [Using quorum authentication for admins: first time setup](quorum-auth-chsm-cli-first-time.md)
+ [Using quorum authentication for admins](quorum-auth-chsm-cli-admin.md)
+ [Change the quorum minimum value for admins](quorum-auth-chsm-cli-min-value.md)

## Overview of quorum authentication with token\-sign strategy<a name="quorum-auth-chsm-cli-overview"></a>

The following steps summarize the quorum authentication processes\. For the specific steps and tools, see [Using quorum authentication for admins](quorum-auth-chsm-cli-admin.md)\.

1. Each HSM user creates an asymmetric key for signing\. Users do this outside of the HSM, taking care to protect the key appropriately\.

1. Each HSM user logs in to the HSM and registers the public part of their signing key \(the public key\) with the HSM\.

1. When an HSM user wants to do a quorum\-controlled operation, the same user logs in to the HSM and gets a *quorum token*\.

1. The HSM user gives the quorum token to one or more other HSM users and asks for their approval\.

1. The other HSM users approve by using their keys to cryptographically sign the quorum token\. This occurs outside the HSM\.

1. When the HSM user has the required number of approvals, the same user logs in to the HSM and runs the quorum\-controlled operation with the \-\-approval argument, supplying the signed quorum token file, which contains all necessary approvals \(signatures\)\.

1. The HSM uses the registered public keys of each signer to verify the signatures\. If the signatures are valid, the HSM approves the token and the quorum\-controlled operation is performed\.

## Additional details about quorum authentication<a name="quorum-authentication-chsm-cli-details"></a>

Note the following additional information about using quorum authentication in AWS CloudHSM\.
+ An HSM user can sign his or her own quorum token—that is, the requesting user can provide one of the required approvals for quorum authentication\.
+ You choose the minimum number of quorum approvers for quorum\-controlled operations\. The smallest number you can choose is two \(2\), and the largest number you can choose is eight \(8\)\.
+ The HSM can store up to 1024 quorum tokens\. If the HSM already has 1024 tokens when you try to create a new one, the HSM purges one of the expired tokens\. By default, tokens expire ten minutes after their creation\.
+ If MFA is enabled, the cluster uses the same key for quorum authentication and for multi\-factor authentication \(MFA\)\. For more information about using quorum authentication and 2FA, see [Using CloudHSM CLI to manage MFA](login-mfa-token-sign.md)\.
+ Each HSM can only contain one token per service at a time\.