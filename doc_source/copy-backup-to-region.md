# Copying backups across AWS Regions<a name="copy-backup-to-region"></a>

 You can copy backups across regions for many reasons, including cross\-region resilience, global workloads, and [disaster recovery](manage-backups.md#recovery-backups)\. After you copy backups, they appear in the destination region with a `CREATE_IN_PROGRESS` status\. Upon successful completion of the copy, the status of the backup changes to `READY`\. If the copy fails, the status of the backup changes to `DELETED`\. Check your input parameters for errors and ensure that the specified source backup is not in a `DELETED` state before rerunning the operation\. For information about backups or how to create a cluster from a backup, see [Managing backups](manage-backups.md) or [Creating clusters from backups](create-cluster-from-backup.md)\. 

 Note the following: 
+ To copy a cluster backup to a destination region, your account must have the proper IAM policy permissions\. In order to copy the backup to a different region, your IAM policy must allow access to the source region in which the backup is located\. Once copied across regions, your IAM policy must allow access to the destination region in order to interact with the copied backup, which includes using the [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html) operation\. For more information, see [Create IAM administrators](create-iam-user.md)\.
+ The original cluster and the cluster that may be built from a backup in the destination region are not linked\. You must manage each of these clusters independently\. For more information, see [Managing clusters](manage-clusters.md)\.
+ Backups cannot be copied between AWS restricted regions and standard regions\. Backups *can* be copied between the AWS GovCloud \(US\-East\) and AWS GovCloud \(US\-West\) regions\.

## Copy backups to different Regions \(console\)<a name="copy-backup-console"></a>

**To copy backups to different Regions \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. In the navigation pane, choose **Backups**\.

1. Choose a backup to copy to a different region\.

1. To copy the selected backup, choose **Actions, Copy backup to another region**\.

   The Copy backup to another region dialog box appears\.

1. In **Destination region**, choose a region from **Select a region**\.

1. \(Optional\) Type a tag key and an optional tag value\. To add more than one tag to the cluster, choose **Add tag**\.

1. Choose **Copy backup**\.

## Copy backups to different Regions \(AWS CLI\)<a name="copy-backups-regions-cli"></a>

To determine the cluster ID or backup ID, run the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) or [describe\-backups](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-backups.html) command respectively\.

**To copy backups to different regions \(AWS CLI\)**
+  At a command prompt, run the [ copy\-backup\-to\-region](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/copy-backup-to-region.html) command\. Specify the destination region and either the cluster ID of the source cluster or the backup ID of the source backup\. If you specify a backup ID, the associated backup is copied\. If you specify a cluster ID, the most recent available backup of the associated cluster is copied\. If you provide both, the backup ID provided is used by default\. 

  ```
  $ aws cloudhsmv2 copy-backup-to-region --destination-region <destination region> \
                                             --backup-id <backup ID>
  {
      "DestinationBackup": {
          "CreateTimestamp": 1531742400,
          "SourceBackup": "backup-4kuraxsqetz",
          "SourceCluster": "cluster-kzlczlspnho",
          "SourceRegion": "us-east-1"
      }
  }
  ```

## Copy backups to different Regions \(AWS CloudHSM API\)<a name="copy-backups-regions-api"></a>

Refer to the following topic to learn how to copy backups to different regions by using the API\.
+  [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CopyBackupToRegion.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CopyBackupToRegion.html) 