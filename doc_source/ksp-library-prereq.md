# Windows AWS CloudHSM Prerequisites<a name="ksp-library-prereq"></a>

Before you can start the Windows AWS CloudHSM client and use the KSP and CNG providers, you must set the required system environment variables\. These variables identify an HSM and a [crypto user](hsm-users.md#crypto-user) \(CU\) for your Windows application\. You can use the [set command](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/set_1) to set temporary system environment variables, or set permanent system environment variables [programmatically](https://msdn.microsoft.com/en-us/library/system.environment.setenvironmentvariable(v=vs.110).aspx) or in the **Advanced** tab of the Windows **System Properties** Control Panel\. 

Set the following system environment variables:

**`n3fips_partition=HSM-ID`**  
Identifies an HSM in your cluster\. Because they are synchronized, you can specify any HSM in the cluster\. To create an HSM, use [CreateHsm](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateHsm.html)\. To find the HSM ID of an HSM, use [DescribeClusters](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeClusters.html) or choose a cluster in the AWS CloudHSM console\.   
For example:  

```
set n3fips_partition=hsm-lgavqitns2a
```

**`n3fips_password=CU-username:CU-password`**  
Identifies a [crypto user](hsm-users.md#crypto-user) \(CU\) in the HSM and provides all required login information\. Your application authenticates and runs as this CU\. The application has the permissions of this CU and can view and manage only the keys that the CU owns and shares\. This CU must be available in the HSM specified by the `n3fips_partition` environment variable\. To create a new CU, use [createUser](cloudhsm_mgmt_util-createUser.md)\. To find existing CUs, use [listUsers](cloudhsm_mgmt_util-listUsers.md)\.  
For example:  

```
set n3fips_password=test_user:password123
```

See also:

[Code Sample for Cavium CNG Provider](ksp-library-sample.md)