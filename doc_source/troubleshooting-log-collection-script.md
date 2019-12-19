# Retrieving Client Configuration Logs<a name="troubleshooting-log-collection-script"></a>

The client log script gathers important environmental information about your operating system, AWS CloudHSM cluster, active or inactive HSMs, and local configuration files\. When you open a case to request assistance with your application, this information helps AWS understand your setup and issue, and helps us troubleshoot your issue faster\.

The script extracts the following information:
+ Operating system and its current version
+ Client configuration information from `cloudhsm_client.cfg`, `cloudhsm_mgmt_util.cfg`, and `application.cfg` files
+ Client logs from the location specific to the platform
+ Cluster and HSM information by using cloudhsm\_mgmt\_util
+ OpenSSL information
+ Current client and build version
+ Installer version

## Running the Client Log Script<a name="running-script"></a>

The script creates an output file with all the gathered information\. You can specify the directory path, where you want to add the output file, as an output parameter in the command\. The directory path must have the appropriate write access\. Alternatively, you can run the script without specifying the directory path\. In such case, the script creates the output file inside the `temp` directory\.

**Linux**: `/opt/cloudhsm/bin/client_info -output`

**Windows**: `C:\Program Files\Amazon\CloudHSM\client_info -output`

Replace the `output` parameter with the directory path where you want to create the output file\.