# Microsoft SignTool with AWS CloudHSM step 3: Sign a file<a name="signtool-sign"></a>

You are now ready to use SignTool and your imported certificate to sign your example file\. In order to do so, you need to know the certificate's SHA\-1 hash, or *thumbprint*\. The thumbprint is used to ensure that SignTool only uses certificates that are verified by AWS CloudHSM\. In this example, we use PowerShell to get the certificate's hash\. You can also use the CA's GUI or the Windows SDK's `certutil` executable\. 

**To obtain a certificate's thumbprint and use it to sign a file**

1. Open PowerShell as an administrator and run the following command:

   ```
   Get-ChildItem -path cert:\LocalMachine\My
   ```

   Copy the `Thumbprint` that is returned\.  
![\[The certificate's hash is returned as the thumbprint\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/signtool-hash.png)

1. Navigate to the directory within PowerShell that contains `SignTool.exe`\. The default location is `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x64`\.

1. Finally, sign your file by running the following command\. If the command is successful, PowerShell returns a success message\.

   ```
   signtool.exe sign /v /fd sha256 /sha1 <thumbprint> /sm /as C:\Users\Administrator\Desktop\<test>.ps1
   ```  
![\[The .ps1 file was successfully signed.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/signtool-last-command.png)

1. \(Optional\) To verify the signature on the file, use the following command:

   ```
   signtool.exe verify /v /pa C:\Users\Administrator\Desktop\<test>.ps1
   ```