# Step 4: Enable HTTPS traffic and verify the certificate<a name="third-offload-linux-jsse-verify"></a><a name="jsse-verify-local-client"></a>

**Test the local client**
+ After replacing the *<variables>* below with your specific data, use the following command to test the Tomcat TLS offload on same instance\.

  ```
  $ curl -sL -w '%{http_code}' -v --cacert <CUSTOM_DIR>/certificate.pem https://localhost 
  ```
  + ***<CUSTOM\_DIR>***: the directory where keystore file is located\.