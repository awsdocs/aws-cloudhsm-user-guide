# help<a name="key_mgmt_util-help"></a>

The help command in key\_mgmt\_util displays information about all available key\_mgmt\_util commands\.

Before you run help, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start)\.

## Syntax<a name="help-syntax"></a>

```
help
```

## Example<a name="help-examples"></a>

This example shows the output of the `help` command\.

**Example**  

```
Command:  help

Help Commands Available:

Syntax: <command> -h


   Command               Description
   =======               ===========

   exit                   Exits this application
   help                   Displays this information

        Configuration and Admin Commands
   getHSMInfo             Gets the HSM Information
   getPartitionInfo       Gets the Partition Information
   listUsers              Lists all users of a partition
   loginStatus            Gets the Login Information
   loginHSM               Login to the HSM
   logoutHSM              Logout from the HSM

        M of N commands
   getToken               Initiate an MxN service and get Token
   delToken               delete Token(s)
   approveToken           Approves an MxN service
   listTokens             List all Tokens in the current partition

        Key Generation Commands

        Asymmetric Keys:
   genRSAKeyPair          Generates an RSA Key Pair
   genDSAKeyPair          Generates a DSA Key Pair
   genECCKeyPair          Generates an ECC Key Pair

        Symmetric Keys:
   genPBEKey              Generates a PBE DES3 key
   genSymKey              Generates a Symmetric keys

        Key Import/Export Commands
   createPublicKey        Creates an RSA public key
   importPubKey           Imports RSA/DSA/EC Public key
   exportPubKey           Exports RSA/DSA/EC Public key
   importPrivateKey       Imports RSA/DSA/EC private key
   exportPrivateKey       Exports RSA/DSA/EC private key
   imSymKey               Imports a Symmetric key
   exSymKey               Exports a Symmetric key
   wrapKey                Wraps a key from from HSM using the specified handle
   unWrapKey              UnWraps a key into HSM using the specified handle

        Key Management Commands
   deleteKey              Delete Key
   setAttribute           Sets an attribute of an object
   getKeyInfo             Get Key Info about shared users/sessions
   findKey                Find Key
   findSingleKey          Find single Key
   getAttribute           Reads an attribute from an object

        Certificate Setup Commands
   getCert                Gets Partition Certificates stored on HSM

        Key Transfer Commands
   insertMaskedObject     Inserts a masked object
   extractMaskedObject    Extracts a masked object

        Management Crypto Commands
   sign                   Generates a signature
   verify                 Verifies a signature
   aesWrapUnwrap          Does NIST AES Wrap/Unwrap

        Helper Commands
   Error2String           Converts Error codes to Strings
                          save key handle in fake PEM format
   getCaviumPrivKey       Saves an RSA private key handle
                          in fake PEM format
   IsValidKeyHandlefile   Checks if private key file has
                          an HSM key handle or a real key
   listAttributes         List all attributes for getAttributes
   listECCCurveIds        List HSM supported ECC CurveIds
```

## Parameters<a name="loginHSM-parameters"></a>

There are no parameters for this command\.

## Related Topics<a name="loginHSM-seealso"></a>
+ [loginHSM and logoutHSM](key_mgmt_util-loginHSM.md)