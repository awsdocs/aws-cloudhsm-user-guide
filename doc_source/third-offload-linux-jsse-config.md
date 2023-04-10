# Step 3: Configure the Tomcat web server<a name="third-offload-linux-jsse-config"></a>

Update your web server software's configuration to use the HTTPS certificate and corresponding fake PEM private key that you created in the previous step\. Remember to backup your existing certificates and keys before you start\. This will finish setting up your Linux web server software for SSL/TLS offload with AWS CloudHSM\.<a name="jsse-config-stop-server"></a>

**Stop the server**
+ After replacing the *<variables>* below with your specific data, run following command to stop Tomcat Server before updating configuration

  ```
  $ /<TOMCAT_DIR>/bin/shutdown.sh
  ```
  + ***<TOMCAT\_DIR>***: Your Tomcat installation directory\.<a name="jsse-config-update-class-path"></a>

**Update Classpath of Tomcat**

1. Connect to your client instance\.

1. Locate tomcat installation folder\.

1. After replacing the *<variables>* below with your specific data, use the following command to add Java library and Cloudhsm Java path in Tomcat classpath, located in Tomcat/bin/catalina\.sh file\.

   ```
   $ sed -i 's@CLASSPATH="$CLASSPATH""$CATALINA_HOME"\/bin\/bootstrap.jar@CLASSPATH="$CLASSPATH""$CATALINA_HOME"\/bin\/bootstrap.jar:'"
           <JAVA_LIB>"'\/*:\/opt\/cloudhsm\/java\/*:.\/*@' <Tomcat_path> /bin/catalina.sh
   ```
   + ***<JAVA\_LIB>***: Java JRE Library location\.
   + ***<Tomcat\_Path>***: Tomcat installation folder\.<a name="jsse-config-add-https"></a>

**Add an HTTPS connector in the server configuration\.**

1. Go to the Tomcat installation folder\.

1. After replacing the *<variables>* below with your specific data, use the following command to add an HTTPS connector to use certificates generated in prerequisites:

   ```
   $ sed -i '/<Connector port="8080"/i <Connector port=\"443\" maxThreads=\"200\" scheme=\"https\" secure=\"true\" SSLEnabled=\"true\" keystoreType=\"CLOUDHSM\" keystoreFile=\"
           <CUSTOM_DIR>/<jsse_keystore_name>.keystore\" keystorePass=\"<KEYSTORE_PASSWORD>\" keyPass=\"<KEY_PASSWORD>
           \" keyAlias=\"<unique_alias_for_key>" clientAuth=\"false\" sslProtocol=\"TLS\"/>' <Tomcat_path>/conf/server.xml
   ```
   + ***<CUSTOM\_DIR>***: Directory where keystore file is located\.
   + ***<jsse\_keystore\_name>***: Name of the Keystore file\.
   + ***<KEYSTORE\_PASSWORD>***: This is the password for your local keystore file\.
   + ***<KEY\_PASSWORD>***: We store reference to your key in the local keystore file, and this password protects that local reference\.
   + ***<unique\_alias\_for\_key>***: This is used to uniquely identify your key on the HSM\. This alias will be set as the LABEL attribute for the key\.
   + ***<Tomcat\_path>***: The path to your Tomcat folder\.<a name="jsse-config-start-server"></a>

**Start Server**
+ After replacing the *<variables>* below with your specific data, use the following command to start Tomcat Server:

  ```
  $ /<TOMCAT_DIR>/bin/startup.sh
  ```
  + ***<TOMCAT\_DIR>***: the name of your Tomcat installation directory\.

After you update your web server configuration, go to [Step 4: Enable HTTPS traffic and verify the certificate](third-offload-linux-jsse-verify.md)\.