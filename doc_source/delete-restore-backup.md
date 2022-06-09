# Deleting and restoring backups<a name="delete-restore-backup"></a>

 After you delete a backup, the service holds the backup for seven days, during which time you can restore the backup\. After the seven\-day period, you can no longer restore the backup\. For more information about managing backups, see [Managing backups](manage-backups.md)\. 

## Delete and restore backups \(console\)<a name="delete-restore-console"></a>

**To delete a backup \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. In the navigation pane, choose **Backups**\.

1. Choose a backup to delete\.

1. To delete the selected backup, choose **Actions, Delete**\.

   The Delete backups dialog box appears\.

1. Choose **Delete**\.

   The state of the back changes to `PENDING_DELETE`\. You can restore a backup that is pending deletion for up to 7 days after you request the deletion\.

**To restore a backup \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. In the navigation pane, choose **Backups**\.

1. Choose a backup in the `PENDING_DELETE` state to restore\.

1. To restore the selected backup, choose **Actions, Restore**\.

## Delete and restore backups \(AWS CLI\)<a name="delete-cli"></a>

Check the status of a backup or find its ID by using the [describe\-backups](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-backups.html) command from the AWS CLI\.

**To delete a backup \(AWS CLI\)**
+  At a command prompt, run the [delete\-backup](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/delete-backup.html) command, passing the ID of the backup to be deleted\. 

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

**To restore a backup \(AWS CLI\)**
+ To restore a backup, issue the [restore\-backup](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/restore-backup.html) command, passing the ID of a backup that is in the `PENDING_DELETION` state\. 

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

**To list backups \(AWS CLI\)**
+  To see a list of all backups in the `PENDING_DELETION` state, run the describe\-backups command and include `states=PENDING_DELETION` as a filter\. 

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

## Delete and restore backups \(AWS CloudHSM API\)<a name="delete-restore-api"></a>

Refer to the following topics to learn how to delete and restore backups by using the API\.
+  [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DeleteBackup.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DeleteBackup.html) 
+ [RestoreBackup](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_RestoreBackup.html) 