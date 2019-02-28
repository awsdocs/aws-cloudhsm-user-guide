# Use Microsoft SignTool with AWS CloudHSM to Sign Files<a name="signtool"></a>

In cryptography and public key infrastructure \(PKI\), digital signatures are used to confirm that data has been sent by a trusted entity\. Signatures also indicate that the data has not been tampered with in transit\. A signature is an encrypted hash that is generated with the sender's private key\. The receiver can verify the data's integrity by decrypting its hash signature with the sender's public key\. In turn, it is the sender's responsibility to maintain a digital certificate\. The digital certificate demonstrates the sender's ownership of the private key and provides the recipient with the public key that is needed for decryption\. As long as the private key is owned by the sender, the signature can be trusted\. AWS CloudHSM provides secure FIPS 140\-2 level 3 validated hardware for you to secure these keys with exclusive single\-tenant access\.

Many organizations use Microsoft SignTool, a command line tool that signs, verifies, and timestamps files to simplify the code signing process\. You can use AWS CloudHSM to securely store your key pairs until they are needed by SignTool, thus creating an easily automatable workflow for signing data\.

The following topics provide an overview of how to use SignTool with AWS CloudHSM:

**Topics**
+ [Microsoft SignTool with AWS CloudHSM Step 1: Set Up the Prerequisites](signtool-prereqs.md)
+ [Microsoft SignTool with AWS CloudHSM Step 2: Create a Signing Certificate](signtool-csr.md)
+ [Microsoft SignTool with AWS CloudHSM Step 3: Sign a File](signtool-sign.md)