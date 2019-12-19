# Associate a AWS CloudHSM Key with a Certificate<a name="ksp-library-associate-key-certificate"></a>

Before you can use AWS CloudHSM keys with third\-party tools, such as Microsoft's [SignTool](https://docs.microsoft.com/en-us/windows/win32/seccrypto/signtool), you must import the key's metadata into the local certificate store and associate the metadata with a certificate\. To import the key's metadata, use the import\_key\.exe utility which is included in CloudHSM version 3\.0 and higher\. The following steps provide additional information, and sample output\.

## Step 1: Import your certificate<a name="import-cert"></a>

On Windows, you should be able to double\-click the certificate to import it to your local certificate store\. 

However, if double\-clicking doesn't work, use the [Microsoft Certreq tool](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn296456%28v%3dws.11%29) to import the certificate into the certificate manager\. For example: 

```
certreq -accept certificatename
```

If this action fails and you receive the error, `Key not found`, continue to Step 2\. If the certificate appears in your key store, you've completed the task and no further action is necessary\.

## Step 2: Gather certificate\-identifying information<a name="cert-identifier"></a>

If the previous step wasn't successful, you'll need to associate your private key with a certificate\. However, before you can create the association, you must first find the certificate's Unique Container Name and Serial Number\. Use a utility, such as certutil, to display the needed certificate information\. The following sample output from certutil shows the container name and the serial number\.

```
================ Certificate 1 ================ Serial Number:
			72000000047f7f7a9d41851b4e000000000004Issuer: CN=Enterprise-CANotBefore: 10/8/2019 11:50
			AM NotAfter: 11/8/2020 12:00 PMSubject: CN=www.example.com, OU=Certificate Management,
			O=Information Technology, L=Seattle, S=Washington, C=USNon-root CertificateCert
			Hash(sha1): 7f d8 5c 00 27 bf 37 74 3d 71 5b 54 4e c0 94 20 45 75 bc 65No key provider
			information Simple container name: CertReq-39c04db0-6aa9-4310-93db-db0d9669f42c Unique
			container name: CertReq-39c04db0-6aa9-4310-93db-db0d9669f42c
```

## Step 3: Associate the AWS CloudHSM private key with the certificate<a name="associate-key-certificate"></a>

To associate the key with the certificate, first be sure to [start the AWS CloudHSM client daemon](key_mgmt_util-getting-started.md#key_mgmt_util-start-cloudhsm-client)\. Then, use import\_key\.exe \(which is included in CloudHSM version 3\.0 and higher\) to associate the private key with the certificate\. When specifying the certificate, use its simple container name\. The following example shows the command and the response\. This action only copies the key's metadata; the key remains on the HSM\.

```
$> import_key.exe –RSA CertReq-39c04db0-6aa9-4310-93db-db0d9669f42c

Successfully opened Microsoft Software Key Storage Provider : 0NCryptOpenKey failed : 80090016
```

## Step 4: Update the certificate store<a name="update-certificate-store"></a>

Be certain the AWS CloudHSM client daemon is still running\. Then, use the certutil verb, \-repairstore, to update the certificate serial number\. The following sample shows the command and output\. See the Microsoft documentation for information about the [\-repairstore verb](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc732443(v=ws.11)?redirectedfrom=MSDN#-repairstore)\.

```
C:\Program Files\Amazon\CloudHSM>certutil -f -csp "Cavium Key Storage Provider"-repairstore my "72000000047f7f7a9d41851b4e000000000004"my "Personal"

================ Certificate 1 ================Serial Number: 72000000047f7f7a9d41851b4e000000000004
Issuer: CN=Enterprise-CA
NotBefore: 10/8/2019 11:50 AM
NotAfter: 11/8/2020 12:00 PM
Subject: CN=www.example.com, OU=Certificate Management, O=Information Technology, L=Seattle, S=Washington, C=US
Non-root CertificateCert Hash(sha1): 7f d8 5c 00 27 bf 37 74 3d 71 5b 54 4e c0 94 20 45 75 bc 65       
SDK Version: 3.0 
Key Container = CertReq-39c04db0-6aa9-4310-93db-db0d9669f42c 
Provider = Cavium Key Storage ProviderPrivate key is NOT exportableEncryption test passedCertUtil: -repairstore command completed successfully.
```

After updating the certificate serial number you can use this certificate and the corresponding AWS CloudHSM private key with any third\-party signing tool on Windows\.