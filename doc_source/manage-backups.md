# Managing AWS CloudHSM backups<a name="manage-backups"></a>

 AWS CloudHSM makes periodic backups of your cluster at least once every 24 hours\. Each backup contains encrypted copies of the following data: 
+ Users \(COs, CUs, and AUs\)
+ Key material and certificates
+ Hardware security module \(HSM\) configuration and policies

 You can't instruct the service to make backups, but you can take certain actions that force the service to create a backup\. The service makes a backup when you perform any of the following actions:
+ Activate a cluster
+ Add an HSM to an active cluster
+ Remove an HSM from an active cluster

AWS CloudHSM deletes backups based on the backup retention policy you set when you create clusters\. For information about managing backup retention policy, see [Configuring backup retention](manage-backup-retention.md)\.

**Topics**
+ [Working with backups](#backups-using)
+ [Deleting and restoring backups](delete-restore-backup.md)
+ [Configuring backup retention](manage-backup-retention.md)
+ [Copying backups across Regions](copy-backup-to-region.md)

## Working with backups<a name="backups-using"></a>

 When you add an HSM to a cluster that previously contained one or more active HSMs, the service restores a backup onto the new HSM\. Use backups to manage a HSM that you use infrequently\. When you don't need the HSM, delete it to trigger a backup\. Later, when you do need the HSM, create a new one in the same cluster, and this action will restore the backup you previously created with the delete HSM operation\. 

### Removing expired keys or inactive users<a name="permenantly-remove-backups"></a>

 You may want to remove unwanted cryptographic materials from your environment such as expired keys or inactive users\. This is a two\-step process\. First, delete these materials from your HSM\. Next, delete all existing backups\. Following this process ensures you do not restore deleted information when initializing a new cluster from backup\. For more information, see [Deleting and restoring backups](delete-restore-backup.md)\. 

### Considering disaster recovery<a name="recovery-backups"></a>

 You can create a cluster from a backup\. You might want to do this to set a recovery point for your cluster\. Nominate a backup that contains all the users, key material, certificates that you want in your recovery point, and then use that backup to create a new cluster\. For more information about creating a cluster from a backup, see [Creating clusters from backups](create-cluster-from-backup.md)\. 

 You can also copy a backup of a cluster into a different region, where you can create a new cluster as a clone of the original\. You may want to do this for a number of reasons, including simplification of the disaster recovery process\. For more information about copying backups to regions, see [Copying backups across Regions](copy-backup-to-region.md)\. 