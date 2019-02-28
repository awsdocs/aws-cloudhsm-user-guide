# Copying a Backup Across Regions<a name="copy-backup-to-region"></a>

AWS CloudHSM allows you to copy a backup of a cluster into a different region, where it can then be used to create a new cluster as a clone of the original\. [AWS CloudHSM Cluster Backups](backups.md) are packages of encrypted data that contain the elements of a particular cluster\. In order to clone a cluster into a different region, first copy the cluster backup to the destination region and then create a new cluster from the copied backup\. You may want to do this for a number of reasons, including simplification of the disaster recovery process\.

You can copy a cluster backup across regions from the [AWS Command Line Interface \(AWS CLI\)](https://aws.amazon.com/cli/), or the AWS CloudHSM API\. Upon issuing the copy\-backup\-to\-region command, the copied backup appears in the destination region with a `CREATE_IN_PROGRESS` status\. Upon successful completion, the status of the copied backup is `READY`\.

In the event that the copy\-backup\-to\-region command cannot be successfully completed, the status of the copied backup is `DELETED`\. Check your input parameters for errors and ensure that the specified source backup is not in a `DELETED` state before rerunning the operation\.

For information on how to create a cluster from a backup, see [Creating an AWS CloudHSM Cluster from a Previous Backup](create-cluster-from-backup.md)\.

**Important**  
Note the following:  
To copy a cluster backup to a destination region, your account must have the proper IAM policy permissions\. In order to copy the backup to a different region, your IAM policy must allow access to the source region in which the backup is located\. Once copied across regions, your IAM policy must allow access to the destination region in order to interact with the copied backup, which includes using the [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html) operation\. For more information, see [Restrict User Permissions to What's Necessary for AWS CloudHSM](create-iam-user.md)\.
The original cluster and the cluster that may be built from a backup in the destination region are not linked\. You must manage each of these clusters independently\. For more information, see [Managing AWS CloudHSM Clusters](manage-clusters.md)
Backups cannot be copied between AWS restricted regions and standard regions\. Backups *can* be copied between the AWS GovCloud \(US\-East\) and AWS GovCloud \(US\-West\) regions\.

**To copy a cluster backup to a different region \(AWS CLI\)**
+ At a command prompt, run the [copy\-backup\-to\-region](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/copy-backup-to-region.html) command\. Specify the destination region and either the cluster ID of the source cluster or the backup ID of the source backup\. If you specify a backup ID, the associated backup is copied\. If you specify a cluster ID, the most recent available backup of the associated cluster is copied\. If you provide both, the backup ID provided is used by default\. If you don't know the cluster ID or backup ID, run the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) or [describe\-backups](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-backups.html) command respectively\.

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

**To copy a cluster backup to a different region \(AWS CloudHSM API\)**
+ Send a [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CopyBackupToRegion.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CopyBackupToRegion.html) request\. Specify the destination region and the cluster ID or most recent backup ID of the cluster to be copied\.