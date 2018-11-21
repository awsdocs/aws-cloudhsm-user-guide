# Deleting and Restoring an AWS CloudHSM Cluster Backup<a name="delete-restore-backup"></a>

[AWS CloudHSM Cluster Backups](backups.md) are packages of encrypted data that contain the elements of a particular cluster\. Backups are generated once a day, as well as whenever an HSM is added to a cluster\.

You may want to remove certain cryptographic materials from your AWS environment, such as an expired key or inactive user\. In order to remove these materials, you first delete them from your HSMs\. To ensure that deleted information is not restored when initializing a new cluster with an old backup, you must then delete all existing backups of the cluster\. To do this, you use the [AWS Command Line Interface \(AWS CLI\)](https://aws.amazon.com/cli/), or the [ AWS CloudHSM API](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/Welcome.html)\.

A backup must be in the `READY` state in order to be deleted\. You can check the status of a backup by using the [describe\-backups](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-backups.html) command from the AWS CLI, or with the [DescribeBackups](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeBackups.html) API call\.

Upon successful deletion, a backup is in the `PENDING_DELETION` state for 7 days, during which time it can be restored with the [restore\-backup](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/restore-backup.html) command\. After the deletion window has passed, the target backup can no longer be restored\.

**To delete and restore a backup \(AWS CLI\)**

1. At a command prompt, run the [delete\-backup](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/delete-backup.html) command, passing the ID of the backup to be deleted\. If you don't know the backup ID, run the [describe\-backups](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-backups.html) command\.

   ```
   $ aws cloudhsmv2 delete-backup --backup-id <backup ID>
   {
       "Backup": {
           "CreateTimestamp": 1534461854.64,
           "ClusterId": "cluster-dygnwhmscg5",
           "BackupId": "backup-ro5c4er4aac",
           "BackupState": "PENDING_DELETION",
           "DeleteTimestamp": 1536339805.522
       }
   }
   ```

1. To restore a backup, issue the [restore\-backup](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/restore-backup.html) command, passing the ID of a backup that is in the `PENDING_DELETION` state\. You can check the status of a backup by issuing the describe\-backups command with the ID of the target backup\. If you would like to see a list of all backups in the `PENDING_DELETION` state, run the describe\-backups command and include `states=PENDING_DELETION` as a filter\.

   ```
   $ aws cloudhsmv2 describe-backups --filters states=PENDING_DELETION
   {
       "Backups": [
           {
               "BackupId": "backup-ro5c4er4aac",
               "BackupState": "PENDING_DELETION",
               "CreateTimestamp": 1534461854.64,
               "ClusterId": "cluster-dygnwhmscg5",
               "DeleteTimestap": 1536339805.522,
           }
   }
   ```

   ```
   $ aws cloudhsmv2 restore-backup --backup-id <backup ID>
   {
       "Backup": {
           "ClusterId": "cluster-dygnwhmscg5",
           "CreateTimestamp": 1534461854.64,
           "BackupState": "READY",
           "BackupId": "backup-ro5c4er4aac"
       }
   }
   ```

**To delete and restore a backup \(AWS CloudHSM API\):**

1. To delete a backup, send a [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DeleteBackup.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DeleteBackup.html) request, specifying the ID of the backup to be deleted\.

1. To restore a backup, send a [RestoreBackup](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_RestoreBackup.html) request, specifying the ID of the backup to be restored\.