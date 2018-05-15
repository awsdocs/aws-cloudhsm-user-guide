# Add and Configure the Windows Server AD CS Role<a name="win-ca-setup"></a>

To add and configure the the Windows Server Active Directory Certificate Services \(AD CS\) role, do the following:

1. Start **Server Manager** on your Windows Server\.

1. Choose **Add Roles and Features** in the **Server Manager** dashboard\.

1. Read the **Before you begin** information and choose **Next**\.

1. For **Installation Type**, choose **Role\-based or feature\-based installation** and then choose **Next**\.

1. For **Server Selection**, choose **Select a server from the server pool** and choose **Next**\.

1. For **Server Roles**:
   + Select **Active Directory Certificate Services** and choose **Next**\.
   + For the **Add features that are required for Active Directory Certificate Services** pop\-up, choose **Add Features**\.
   + Choose **Next** to finish selecting additional features\.
   + Choose **Next** to finish selecting server roles\.

1. For **Features**, accept the defaults and choose **Next**\.

1. For **AD CA**:
   + Choose **Next**\.
   + Select **Certification Authority**\.
   + Choose **Next**\.

1. For **Confirmation**, read the information presented and choose **Install**\. Do not close the window\.

1. Choose the highlighted **Configure Active Directory Certificate Services on the destination server** link\.

1. For **Credentials**, accept or change the credentials displayed and click **Next**\.

1. For **Role Services**, select **Certification Authority **and choose **Next**\.

1. For **Setup Type**, select **Standalone CA** and choose **Next**\.

1. For **CA Type**, select **Root CA** and choose **Next**\.

1. For **Private Key**, select **Create a new private key** and choose **Next**\.
   + For **Select a cryptographic provider**, choose a **Cavium Key Storage Provider** from the dropdown\.
   + Choose a **Key length**\.
   + Select a hash algorithm for signing certificates issued by the CA\.
   + Choose **Next**\.

1. For **CA Name**:
   + Accept or edit the common name\.
   + Type an optional distinguished name suffix\.
   + Verify the preview of the distinguished name\.
   + Choose **Next**\.

1. For **Validity Period**, specify a time period in years, weeks, days, or months\.

1. For **Certificate Database**:
   + Type the database location and the database log location or accept the default values\.
   + Choose **Next**\.

1. For **Confirmation**, review the information provided and choose **Configure**\.

1. Choose **Close**\.