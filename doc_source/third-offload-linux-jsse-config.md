# Step 3: Configure the Tomcat web server<a name="third-offload-linux-jsse-config"></a>

Update your web server software's configuration to use the HTTPS certificate and corresponding PEM file that you created in the previous step\. Remember to backup your existing certificates and keys before you start\. This will finish setting up your Linux web server software for SSL/TLS offload with AWS CloudHSM\. For more information, refer to the [Apache Tomcat 9 Configuration Reference](https://tomcat.apache.org/tomcat-9.0-doc/config/http.html)\.<a name="jsse-config-stop-server"></a>

**Stop the server**
+ After replacing the *<VARIABLES>* below with your specific data, run following command to stop Tomcat Server before updating configuration

  ```
  $ /<TOMCAT DIRECTORY>/bin/shutdown.sh
  ```
  + ***<TOMCAT DIRECTORY>***: Your Tomcat installation directory\.<a name="jsse-config-update-class-path"></a>

**Update Classpath of Tomcat**

1. Connect to your client instance\.

1. Locate the Tomcat installation folder\.

1. After replacing the *<VARIABLES>* below with your specific data, use the following command to add Java library and Cloudhsm Java path in Tomcat classpath, located in Tomcat/bin/catalina\.sh file\.

   ```
   $ sed -i 's@CLASSPATH="$CLASSPATH""$CATALINA_HOME"\/bin\/bootstrap.jar@CLASSPATH="$CLASSPATH""$CATALINA_HOME"\/bin\/bootstrap.jar:'"
           <JAVA LIBRARY>"'\/*:\/opt\/cloudhsm\/java\/*:.\/*@' <TOMCAT PATH> /bin/catalina.sh
   ```
   + ***<JAVA LIBRARY>***: Java JRE Library location\.
   + ***<TOMCAT PATH>***: Tomcat installation folder\.<a name="jsse-config-add-https"></a>

**Add an HTTPS connector in the server configuration\.**

1. Go to the Tomcat installation folder\.

1. After replacing the *<VARIABLES>* below with your specific data, use the following command to add an HTTPS connector to use certificates generated in prerequisites:

   ```
   $ sed -i '/<Connector port="8080"/i <Connector port=\"443\" maxThreads=\"200\" scheme=\"https\" secure=\"true\" SSLEnabled=\"true\" keystoreType=\"CLOUDHSM\" keystoreFile=\"
           <CUSTOM DIRECTORY>/<JSSE KEYSTORE NAME>.keystore\" keystorePass=\"<KEYSTORE PASSWORD>\" keyPass=\"<KEY PASSWORD>
           \" keyAlias=\"<UNIQUE ALIAS FOR KEYS>" clientAuth=\"false\" sslProtocol=\"TLS\"/>' <TOMCAT PATH>/conf/server.xml
   ```
   + ***<CUSTOM DIRECTORY>***: Directory where keystore file is located\.
   + ***<JSSE KEYSTORE NAME>***: Name of the Keystore file\.
   + ***<KEYSTORE PASSWORD>***: This is the password for your local keystore file\.
   + ***<KEY PASSWORD>***: We store reference to your key in the local keystore file, and this password protects that local reference\.
   + ***<UNIQUE ALIAS FOR KEYS>***: This is used to uniquely identify your key on the HSM\. This alias will be set as the LABEL attribute for the key\.
   + ***<TOMCAT PATH>***: The path to your Tomcat folder\.<a name="jsse-config-start-server"></a>

**Start Server**
+ After replacing the *<VARIABLES>* below with your specific data, use the following command to start Tomcat Server:

  ```
  $ /<TOMCAT DIRECTORY>/bin/startup.sh
  ```
  + ***<TOMCAT DIRECTORY>***: the name of your Tomcat installation directory\.

After you update your web server configuration, go to [Step 4: Enable HTTPS traffic and verify the certificate](third-offload-linux-jsse-verify.md)\.