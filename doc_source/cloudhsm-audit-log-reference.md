# Audit Log Reference<a name="cloudhsm-audit-log-reference"></a>

AWS CloudHSM records HSM management commands in audit log events\. Each event has an operation code \(`Opcode`\) value that identifies the action that occurred and its response\. You can use the `Opcode` values to search, sort, and filter the logs\.

The following table defines the `Opcode` values in an AWS CloudHSM audit log\.


| Operation Code \(Opcode\) | Description | 
| --- | --- | 
| User Login: These events include the user name and user type\. | 
| CN\_LOGIN \(0xd\) | [User login](cloudhsm_mgmt_util-loginLogout.md) \(excludes appliance user \[AU\]\)\. | 
| CN\_LOGOUT \(0xe\) | [User logout](cloudhsm_mgmt_util-loginLogout.md) \(excludes appliance user \[AU\]\)\. | 
| CN\_APP\_FINALIZE | App finalize \(logged only when user did not explicitly log out\) | 
| CN\_CLOSE\_SESSION | Close session \(logged only when user did not explicitly log out\) | 
| User Management: These events include the user name and user type\. | 
| CN\_CREATE\_USER \(0x3\) | [Create a crypto user \(CU\)](cloudhsm_mgmt_util-createUser.md) | 
| CN\_CREATE\_CO | [Create a crypto officer \(CO\)](cloudhsm_mgmt_util-createUser.md) | 
| CN\_CREATE\_APPLIANCE\_USER | [Create an appliance user \(AU\)](cloudhsm_mgmt_util-createUser.md) | 
| CN\_DELETE\_USER | [Delete a user](cloudhsm_mgmt_util-deleteUser.md) | 
| CN\_CHANGE\_PSWD | [Change a user password](cloudhsm_mgmt_util-changePswd.md) | 
| CN\_SET\_M\_VALUE | Set quorum authentication \(M of N\) for a user action\. | 
| CN\_APPROVE\_TOKEN | Approve a quorum authentication token for a user action\. | 
| Key Management: These events include the key handle\.  | 
| CN\_GENERATE\_KEY | [Generate a symmetric key](key_mgmt_util-genSymKey.md) | 
| CN\_GENERATE\_KEY\_PAIR \(0x19\) | Generate a key pair \([DSA](key_mgmt_util-genDSAKeyPair.md), [ECC](key_mgmt_util-genECCKeyPair.md), or [RSA](key_mgmt_util-genRSAKeyPair.md)\) | 
| CN\_CREATE\_OBJECT | Import a public key \(without wrapping\) | 
| CN\_MODIFY\_OBJECT | Set a key attribute in [key\_mgmt\_util](key_mgmt_util-setAttribute.md) or [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-setAttribute.md)\. | 
| CN\_DESTROY\_OBJECT \(0x11\) | [Delete a key](key_mgmt_util-deleteKey.md) | 
| CN\_TOMBSTONE\_OBJECT | Mark the key for deletion, but do not remove it | 
| CN\_SHARE\_OBJECT | [Share or unshare a key](cloudhsm_mgmt_util-shareKey.md) | 
| CN\_WRAP\_KEY | Export an encrypted copy of a key \([wrapKey](key_mgmt_util-wrapKey.md)\) | 
| CN\_UNWRAP\_KEY | Import an encrypted copy of a key \([unwrapKey](key_mgmt_util-unwrapKey.md)\) | 
| CN\_NIST\_AES\_WRAP |  Encrypt or decrypt a file \([aesWrapUnwrap](key_mgmt_util-aesWrapUnwrap.md)\)  | 
| CN\_INSERT\_MASKED\_OBJECT\_USER | Receive a key \(as a masked object\) from another HSM in the cluster; this event is recorded when a client action synchronizes the key | 
| CN\_EXTRACT\_MASKED\_OBJECT\_USER | Send a key \(as a masked object\) to other HSMs in the cluster; this event is recorded when a client action synchronizes the key | 
| Clone HSMs | 
| CN\_CLONE\_SOURCE\_INIT | Clone source start | 
| CN\_CLONE\_SOURCE\_STAGE1 | Clone source end | 
| CN\_CLONE\_TARGET\_INIT | Clone target start | 
| CN\_CLONE\_TARGET\_STAGE1 | Clone target end | 
| Certificate\-Based Authentication | 
| CN\_CERT\_AUTH\_STORE\_CERT | Store a certificate | 
| CN\_CERT\_AUTH\_VALIDATE\_PEER\_CERTS | Validate a certificate | 
| CN\_CERT\_AUTH\_SOURCE\_KEY\_EXCHANGE | Source key exchange | 
| CN\_CERT\_AUTH\_TARGET\_KEY\_EXCHANGE | Target key exchange | 
| HSM Instance Commands | 
| CN\_INIT\_TOKEN \(0x1\) | Initialize the HSM: Start | 
| CN\_INIT\_DONE | Initialize the HSM: Complete | 
| CN\_GEN\_KEY\_ENC\_KEY | Generate a key encryption key \(KEK\) | 
| CN\_GEN\_PSWD\_ENC\_KEY \(0x1d\) | Generate a password encryption key \(PEK\) | 
| CN\_CLOSE\_PARTITION\_SESSIONS | Close a session on the HSM | 
| CN\_STORE\_KBK\_SHARE | Store the key backup key \(KBK\) | 
| CN\_SET\_NODEID | Set the node ID of the HSM in the cluster | 
| CN\_ZEROIZE | Zeroize the HSM | 