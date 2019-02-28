# Microsoft SignTool with AWS CloudHSM Step 2: Create a Signing Certificate<a name="signtool-csr"></a>

Now that you've downloaded the Windows SDK on to your EC2 instance, you can use it to generate a certificate signing request \(CSR\)\. The CSR is an unsigned certificate that is eventually passed to your CA for signing\. In this example, we use the `certreq` executable that's included with the Windows SDK to generate the CSR\.

**To generate a CSR using the `certreq` executable**

1. If you haven't already done so, connect to your Windows EC2 instance\. For more information, see [Connect to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows) in the *Amazon EC2 User Guide for Windows Instances*\.

1. Create a file called `request.inf` that contains the lines below\. Replace the `Subject` information with that of your organization\. For an explanation of each parameter, see [Microsoft's documentation](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/certreq_1#BKMK_New)\.

   ```
   [Version]
   Signature= $Windows NT$
   [NewRequest]
   Subject = "C=<Country>,CN=<www.website.com>,\
              O=<Organization>,OU=<Organizational-Unit>,\
              L=<City>,S=<State>"
   RequestType=PKCS10
   HashAlgorithm = SHA256
   KeyAlgorithm = RSA
   KeyLength = 2048
   ProviderName = Cavium Key Storage Provider
   KeyUsage = "CERT_DIGITAL_SIGNATURE_KEY_USAGE"
   MachineKeySet = True
   Exportable = False
   ```

1. Run `certreq.exe`\. For this example, we save the CSR as `request.csr`\.

   ```
   certreq.exe -new request.inf request.csr
   ```

   Internally, a new key pair is generated on your AWS CloudHSM cluster, and the pair's private key is used to create the CSR\.

1. Submit the CSR to your CA\. If you are using a Windows Server CA, follow these steps:

   1. Enter the following command to open the CA tool:

      ```
      certsrv.msc
      ```

   1. In the new window, right\-click the CA server's name\. Choose **All Tasks**, and then choose **Submit new request**\.

   1. Navigate to `request.csr`'s location and choose **Open**\.

   1. Navigate to the **Pending Requests** folder by expanding the **Server CA** menu\. Right\-click on the request you just created, and under **All Tasks** choose **Issue**\.

   1. Now navigate to the **Issued Certificates** folder \(above the **Pending Requests** folder\)\.

   1. Choose **Open** to view the certificate, and then choose the **Details** tab\.

   1. Choose **Copy to File** to start the Certificate Export Wizard\. Save the DER\-encoded X\.509 file to a secure location as `signedCertificate.cer`\.

   1. Exit the CA tool and use the following command, which moves the certificate file to the Personal Certificate Store in Windows\. It can then be used by other applications\.

      ```
      certreq.exe -accept signedCertificate.cer
      ```

You can now use your imported certificate to [Sign a File ](signtool-sign.md)\.