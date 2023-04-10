# Known issues for integrating third\-party applications<a name="ki-third-party"></a>

## Issue: Client SDK 3 does not support Oracle setting PKCS \#11 attribute `CKA_MODIFIABLE` during master key generation<a name="ki-third-party-1"></a>

This limit is defined in the PKCS \#11 library\. For more information, see annotation 1 on [Supported PKCS \#11 Attributes](pkcs11-attributes.md)\. 
+ **Impact:** Oracle master key creation fails\.
+ **Workaround:** Set the special environment variable `CLOUDHSM_IGNORE_CKA_MODIFIABLE_FALSE` to TRUE when creating a new master key\. This environment variable is only needed for master key generation and you do not need to use this environment variable for anything else\. For example, you would use this variable for the first master key you create and then you would only use this environment variable again if you wanted to rotate your master key edition\. For more information, see [Generate the Oracle TDE Master Encryption Key](oracle-tde-configure-database-and-generate-master-key.md#oracle-tde-generate-master-key)\.
+ **Resolution status:** We are improving the HSM firmware to fully support the CKA\_MODIFIABLE attribute\. Updates will be announced in the AWS CloudHSM forum and on the version history page