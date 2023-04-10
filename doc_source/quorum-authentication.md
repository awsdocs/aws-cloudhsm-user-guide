# Using CloudHSM Management Utility \(CMU\) to manage quorum authentication \(M of N access control\)<a name="quorum-authentication"></a>

The HSMs in your AWS CloudHSM cluster support quorum authentication, which is also known as M of N access control\. With quorum authentication, no single user on the HSM can do quorum\-controlled operations on the HSM\. Instead, a minimum number of HSM users \(at least 2\) must cooperate to do these operations\. With quorum authentication, you can add an extra layer of protection by requiring approvals from more than one HSM user\.

Quorum authentication can control the following operations:
+ HSM user management by [crypto officers \(COs\)](manage-hsm-users-cmu.md#crypto-officer) – Creating and deleting HSM users, and changing a different HSM user's password\. For more information, see [Using quorum authentication for crypto officers](quorum-authentication-crypto-officers.md)\.

The following topics provide more information about quorum authentication in AWS CloudHSM\.

**Topics**
+ [Overview of quorum authentication](#quorum-authentication-overview)
+ [Additional details about quorum authentication](#quorum-authentication-details)
+ [Using quorum authentication for crypto officers: first time setup](quorum-authentication-crypto-officers-first-time-setup.md)
+ [Using quorum authentication for crypto officers](quorum-authentication-crypto-officers.md)
+ [Change the quorum minimum value for crypto officers](quorum-authentication-crypto-officers-change-minimum-value.md)

## Overview of quorum authentication<a name="quorum-authentication-overview"></a>

The following steps summarize the quorum authentication processes\. For the specific steps and tools, see [Using quorum authentication for crypto officers](quorum-authentication-crypto-officers.md)\.

1. Each HSM user creates an asymmetric key for signing\. They do this outside of the HSM, taking care to protect the key appropriately\.

1. Each HSM user logs in to the HSM and registers the public part of their signing key \(the public key\) with the HSM\.

1. When an HSM user wants to do a quorum\-controlled operation, each user logs in to the HSM and gets a *quorum token*\.

1. The HSM user gives the quorum token to one or more other HSM users and asks for their approval\.

1. The other HSM users approve by using their keys to cryptographically sign the quorum token\. This occurs outside the HSM\.

1. When the HSM user has the required number of approvals, the same user logs in to the HSM and gives the quorum token and approvals \(signatures\) to the HSM\.

1. The HSM uses the registered public keys of each signer to verify the signatures\. If the signatures are valid, the HSM approves the token\.

1. The HSM user can now do a quorum\-controlled operation\.

## Additional details about quorum authentication<a name="quorum-authentication-details"></a>

Note the following additional information about using quorum authentication in AWS CloudHSM\.
+ An HSM user can sign their own quorum token—that is, the requesting user can provide one of the required approvals for quorum authentication\.
+ You choose the minimum number of quorum approvers for quorum\-controlled operations\. The smallest number you can choose is two \(2\), and the largest number you can choose is eight \(8\)\.
+ The HSM can store up to 1024 quorum tokens\. If the HSM already has 1024 tokens when you try to create a new one, the HSM purges one of the expired tokens\. By default, tokens expire ten minutes after their creation\.
+ The cluster uses the same key for quorum authentication and for two\-factor authentication \(2FA\)\. For more information about using quorum authentication and 2FA, see [Quorum Authentication and 2FA](manage-2fa.md#quorum-2fa)\.