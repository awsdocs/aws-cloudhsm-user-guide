# Best practices for AWS CloudHSM<a name="best-practices"></a>

Learn best practices for working with AWS CloudHSM\. As new best practices are identified, we'll update this section\.

## AWS CloudHSM basic operational guidelines<a name="base-operations"></a>

Use the following guidelines when working with AWS CloudHSM\. The AWS CloudHSM Service Level Agreement requires that you follow these guidelines\.

### <a name="w13aab9b5b5"></a>

**Administration**: We recommend you create at least two admins/cryptographic officers \(COs\) to administer your cluster\. Before setting quorum \(MofN\) policy, you must create at least M\+1 admin/CO accounts\. Delete admins/CO accounts with caution\. If you fall below the quorum number of admins/COs, you will no longer be able to administer your cluster\.

For more information, refer to [Using CloudHSM CLI to manage quorum authentication \(M of N access control\)](quorum-auth-chsm-cli.md)\.

### <a name="w13aab9b5b7"></a>

**Cluster Configuration**: For production clusters, you should have at least two HSM instances spread across two availability zones in a region\. For latency\-sensitive workloads, we recommend \+1 redundancy\. For applications requiring durability of newly generated keys, we recommend at least three HSM instances spread across all availability zones in a region\.

For more information, refer to [Managing AWS CloudHSM clusters](manage-clusters.md)\.

### <a name="w13aab9b5b9"></a>

**Cluster Capacity**: You should create enough HSMs in your cluster to handle your workload\. You should consider both audit log capacity and cryptographic capacity when deciding how many HSMs to create in a cluster\.

For more information, refer to [Managing AWS CloudHSM clusters](manage-clusters.md)\.

### <a name="best-practices-key-protection"></a>

**Protecting keys and extracting keys from an HSM**: AWS CloudHSM let’s you choose which cleartext keys can be extracted and not extracted, so it is important that you identify which should be extractable and which should not be\. Once identified, use each key’s attributes to ensure they are set correctly:
+ Keys with their `WRAP_WITH_TRUSTED` attribute do not allow keys to be exported in cleartext, and is suitable for a variety of use cases\. For more information see Using trusted keys to [control key unwraps](cloudhsm_using_trusted_keys_control_key_wrap.md)\.
+ Keys with their `EXTRACTABLE` attribute set to false can never leave the HSM, and there is no way to unset this property after it has been set\.