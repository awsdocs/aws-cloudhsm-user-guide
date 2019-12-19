# Best Practices for AWS CloudHSM<a name="best-practices"></a>

Learn best practices for working with AWS CloudHSM\. As new best practices are identified, we'll update this section\.

## AWS CloudHSM Basic Operational Guidelines<a name="base-operations"></a>

Use the following guidelines when working with AWS CloudHSM\. The AWS CloudHSM Service Level Agreement requires that you follow these guidelines\.

**Administration**: We recommend you create at least two cryptographic officers \(COs\) to administer your cluster\. Before setting quorum \(MofN\) policy, you must create at least M\+1 CO accounts\. Delete CO accounts with caution\. If you fall below the quorum number of COs, you will no longer be able to administer your cluster\.

**Administration**: If an HSM fails, you can experience unrecoverable data loss\. We do not configure fault tolerance for you\. You are responsible for configuring fault tolerance for your HSMs\.

**Cluster Configuration**: For production clusters, you should have at least two HSM instances spread across two availability zones in a region\. For latency\-sensitive workloads, we recommend \+1 redundancy\. For applications requiring durability of newly generated keys, we recommend at least three HSM instances spread across all availability zones in a region\.

**Cluster Capacity**: You should create enough HSMs in your cluster to handle your workload\. You should consider both audit log capacity and cryptographic capacity when deciding how many HSMs to create in a cluster\.