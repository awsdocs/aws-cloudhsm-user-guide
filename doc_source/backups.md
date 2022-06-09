# AWS CloudHSM cluster backups<a name="backups"></a>

![\[AWS CloudHSM cluster backups encrypted in a service-controlled Amazon S3 bucket.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/cluster-backup.png)

AWS CloudHSM makes periodic backups of the users, keys, and policies in the cluster\. The service stores backups in a service\-controlled Amazon Simple Storage Service \(Amazon S3\) bucket in the same region as your cluster\. The preceding illustration shows the relationship of your backups to the cluster\. Backups are secure, durable, and updated on a predictable schedule\. For more information about the security and durability of backups, see the following sections\. For more information about working with backups, see [Managing backups](manage-backups.md)\. 

## Security of backups<a name="backup-security"></a>

When AWS CloudHSM makes a backup from the HSM, the HSM encrypts all of its data before sending it to AWS CloudHSM\. The data never leaves the HSM in plaintext form\.

To encrypt its data, the HSM uses a unique, ephemeral encryption key known as the ephemeral backup key \(EBK\)\. The EBK is an AES 256\-bit encryption key generated inside the HSM when AWS CloudHSM makes a backup\. The HSM generates the EBK, then uses it to encrypt the HSM's data with a FIPS\-approved AES key wrapping method that complies with [NIST special publication 800\-38F](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-38F.pdf)\. Then the HSM gives the encrypted data to AWS CloudHSM\. The encrypted data includes an encrypted copy of the EBK\.

To encrypt the EBK, the HSM uses another encryption key known as the persistent backup key \(PBK\)\. The PBK is also an AES 256\-bit encryption key\. To generate the PBK, the HSM uses a FIPS\-approved key derivation function \(KDF\) in counter mode that complies with [NIST special publication 800\-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)\. The inputs to this KDF include the following:
+ A manufacturer key backup key \(MKBK\), permanently embedded in the HSM hardware by the hardware manufacturer\.
+ An AWS key backup key \(AKBK\), securely installed in the HSM when it's initially configured by AWS CloudHSM\.

The encryption processes are summarized in the following figure\. The backup encryption key represents the persistent backup key \(PBK\) and the ephemeral backup key \(EBK\)\. 

![\[A summary of the encryption keys that are used to encrypt AWS CloudHSM backups.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/backup-security.png)

AWS CloudHSM can restore backups onto only AWS\-owned HSMs made by the same manufacturer\. Because each backup contains all users, keys, and configuration from the original HSM, the restored HSM contains the same protections and access controls as the original\. The restored data overwrites all other data that might have been on the HSM prior to restoration\.

A backup consists of only encrypted data\. Before the service stores a backup in Amazon S3, the service encrypts the backup again using AWS Key Management Service \(AWS KMS\)\.

## Durability of backups<a name="backups-durability"></a>

AWS CloudHSM stores cluster backups in an Amazon S3 bucket that the service controls\. Backups have a 99\.999999999% durability level, the same as any object stored in Amazon S3\. 