# Creating AWS CloudHSM clusters from backups<a name="create-cluster-from-backup"></a>

To restore an AWS CloudHSM cluster from a backup, create a cluster and specify the backup to restore\. After you create the cluster, don't initialize or activate it\. Add a HSM to the cluster and your cluster will contain the same users, key material, certificates, configuration, and policies that were in the backup\. For more information about managing backups, see [Managing backups](manage-backups.md)\. 

## Create clusters from backups \(console\)<a name="create-cluster-backup-console"></a>

**To create a cluster from a previous backup \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. Choose **Create cluster**\.

1. In the **Cluster configuration** section, do the following:

   1. For **VPC**, choose a VPC for the cluster that you are creating\.

   1. For **AZ\(s\)**, choose a private subnet for each Availability Zone that you are adding to the cluster\.

1. In the **Cluster source** section, do the following:

   1. Choose **Restore cluster from existing backup**\.

   1. Choose the backup that you are restoring\.

1. Choose **Next: Review**\.

1. Review your cluster configuration, then choose **Create cluster**\.

1. Specify how long the service should retain backups\.

   Accept the default retention period of 90 days or type a new value between 7 and 379 days\. The service will automatically delete backups in this cluster older than the value you specify here\. You can change this later\. For more information, see [Configuring backup retention](manage-backup-retention.md)\.

1. Choose **Next**\.

1. \(Optional\) Type a tag key and an optional tag value\. To add more than one tag to the cluster, choose **Add tag**\.

1. Choose **Review**\.

1. Review your cluster configuration, and then choose **Create cluster**\.

**Tip**  
To create a HSM in this cluster that contains the same users, key material, certificates, configuration, and policies that were in the backup that you restored, [add a HSM](add-remove-hsm.md#add-hsm) to the cluster\.

## Create clusters from backups \(AWS CLI\)<a name="create-cluster-backup-cli"></a>

To determine the backup ID, issue the [describe\-backups](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-backups.html) command\.

**To create clusters from backups \(AWS CLI\)**
+  At a command prompt, issue the [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/create-cluster.html) command\. Specify the HSM instance type, the subnet IDs of the subnets where you plan to create HSMs, and the backup ID of the backup that you are restoring\. 

  ```
  $ aws cloudhsmv2 create-cluster --hsm-type hsm1.medium \
                                  --subnet-ids <subnet ID 1> <subnet ID 2> <subnet ID N> \
                                  --source-backup-id <backup ID>
  {
      "Cluster": {
          "HsmType": "hsm1.medium",
          "VpcId": "vpc-641d3c0d",
          "Hsms": [],
          "State": "CREATE_IN_PROGRESS",
          "SourceBackupId": "backup-rtq2dwi2gq6",
          "BackupPolicy": "DEFAULT",
          "BackupRetentionPolicy": {
              "Type": "DAYS",
              "Value": 90
           }, 
          "SecurityGroup": "sg-640fab0c",
          "CreateTimestamp": 1504907311.112,
          "SubnetMapping": {
              "us-east-2c": "subnet-0e358c43",
              "us-east-2a": "subnet-f1d6e798",
              "us-east-2b": "subnet-40ed9d3b"
          },
          "Certificates": {
              "ClusterCertificate": "<certificate string>"
          },
          "ClusterId": "cluster-jxhlf7644ne"
      }
  }
  ```

## Create clusters from backups \(AWS CloudHSM API\)<a name="create-cluster-backup-api"></a>

Refer to the following topic to learn how to create clusters from backups by using the API\.
+  [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html) 