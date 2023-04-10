# getHSMInfo<a name="cloudhsm_mgmt_util-getHSMInfo"></a>

The getHSMInfo command in cloudhsm\_mgmt\_util gets information about the hardware on which each HSM runs, including the model, serial number, FIPS state, memory, temperature, and the version numbers of the hardware and firmware\. The information also includes the server ID that cloudhsm\_mgmt\_util uses to refer to the HSM\.

Before you run any CMU command, you must start CMU and log in to the HSM\. Be sure that you log in with a user type that can run the commands you plan to use\.

If you add or delete HSMs, update the configuration files for CMU\. Otherwise, the changes that you make might not be effective for all HSMs in the cluster\.

## User type<a name="getHSMInfo-userType"></a>

The following types of users can run this command\.
+ All users\. You do not have to be logged in to run this command\.

## Syntax<a name="getHSMInfo-syntax"></a>

This command has no parameters\.

```
getHSMInfo
```

## Example<a name="getHSMInfo-examples"></a>

This example uses getHSMInfo to get information about the HSMs in the cluster\.

```
aws-cloudhsm> getHSMInfo
Getting HSM Info on 3 nodes
                *** Server 0 HSM Info ***

        Label                :cavium
        Model                :NITROX-III CNN35XX-NFBE

        Serial Number        :3.0A0101-ICM000001
        HSM Flags            :0
        FIPS state           :2 [FIPS mode with single factor authentication]

        Manufacturer ID      :
        Device ID            :10
        Class Code           :100000
        System vendor ID     :177D
        SubSystem ID         :10


        TotalPublicMemory    :560596
        FreePublicMemory     :294568
        TotalPrivateMemory   :0
        FreePrivateMemory    :0

        Hardware Major       :3
        Hardware Minor       :0

        Firmware Major       :2
        Firmware Minor       :03

        Temperature          :56 C

        Build Number         :13

        Firmware ID          :xxxxxxxxxxxxxxx

...
```

## Related topics<a name="getHSMInfo-seealso"></a>
+ [info](cloudhsm_mgmt_util-info.md)