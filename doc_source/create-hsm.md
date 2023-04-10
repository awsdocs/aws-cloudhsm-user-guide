# Create an HSM<a name="create-hsm"></a>

After you create a cluster, you can create an HSM\. However, before you can create an HSM in your cluster, the cluster must be in the uninitialized state\. To determine the cluster's state, view the [clusters page in the AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/home), use the AWS CLI to run the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command, or send a [DescribeClusters](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeClusters.html) request in the AWS CloudHSM API\. You can create an HSM from the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), the [AWS CLI](https://aws.amazon.com/cli/), or the AWS CloudHSM API\. 



**To create an HSM \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/home](https://console.aws.amazon.com/cloudhsm/home)\.

1. Select the radio button next to the ID of the cluster you want to create an HSM for\.

1. Select **Actions**\. From the drop down menu, choose **Initialize**\.

1. Choose an Availability Zone \(AZ\) for the HSM that you are creating\.

1. Select **Create**\.

**To create an HSM \([AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/)\)**
+ At a command prompt, run the [create\-hsm](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/create-hsm.html) command\. Specify the cluster ID of the cluster that you created previously and an Availability Zone for the HSM\. Specify the Availability Zone in the form of `us-west-2a`, `us-west-2b`, etc\.

  ```
  $ aws cloudhsmv2 create-hsm --cluster-id <cluster ID> --availability-zone <Availability Zone>
  
  {
      "Hsm": {
          "HsmId": "hsm-ted36yp5b2x",
          "EniIp": "10.0.1.12",
          "AvailabilityZone": "us-west-2a",
          "ClusterId": "cluster-igklspoyj5v",
          "EniId": "eni-5d7ade72",
          "SubnetId": "subnet-fd54af9b",
          "State": "CREATE_IN_PROGRESS"
      }
  }
  ```

**To create an HSM \(AWS CloudHSM API\)**
+ Send a [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateHsm.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateHsm.html) request\. Specify the cluster ID of the cluster that you created previously and an Availability Zone for the HSM\. 

After you create a cluster and HSM, you can optionally [verify the identity of the HSM](verify-hsm-identity.md), or proceed directly to [Initialize the cluster](initialize-cluster.md)\.