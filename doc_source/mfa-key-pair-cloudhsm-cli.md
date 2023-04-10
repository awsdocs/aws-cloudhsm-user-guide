# MFA Key Pair Requirements<a name="mfa-key-pair-cloudhsm-cli"></a>

To enable MFA for an HSM user, you can create a new key pair or use an existing key that meets the following requirements:
+ **Key type:** Asymmetric
+ **Key usage:** Sign and verify
+ **Key spec:** RSA\_2048
+ **Signing algorithm includes:** sha256WithRSAEncryption

**Note**  
If you are using quorum authentication or plan to use quorum authentication, see [Quorum authentication and MFA](understand-mfa-cloudhsm-cli.md#quorum-mfa-cloudhsm-cli)

You can use CloudHSM CLI and the key pair to create a new admin user with MFA enabled\.