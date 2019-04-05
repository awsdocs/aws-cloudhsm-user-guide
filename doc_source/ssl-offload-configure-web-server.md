# Step 3: Configure the Web Server<a name="ssl-offload-configure-web-server"></a>

Update your web server software's configuration to use the HTTPS certificate and corresponding fake PEM private key that you created in the [previous step](ssl-offload-import-or-generate-private-key-and-certificate.md)\. This will finish setting up your Linux web server software for SSL/TLS offload with AWS CloudHSM\.

To update your web server configuration, complete the steps from one of the following procedures\. Choose the procedure that corresponds to your web server software\. 
+ [Update the configuration for NGINX](#update-web-server-config-nginx)
+ [Update the configuration for Apache HTTP Server](#update-web-server-config-apache)<a name="update-web-server-config-nginx"></a>

**To update the web server configuration for NGINX**

1. Connect to your client instance\.

1. Run the following command to create the required directories for the web server certificate and the fake PEM private key\. 

   ```
   sudo mkdir -p /etc/pki/nginx/private
   ```

1. Run the following command to copy your web server certificate to the required location\. Replace *<web\_server\.crt>* with the name of your web server certificate\.

   ```
   sudo cp <web_server.crt> /etc/pki/nginx/server.crt
   ```

1. Run the following command to copy your fake PEM private key to the required location\. Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

   ```
   sudo cp <web_server_fake_PEM.key> /etc/pki/nginx/private/server.key
   ```

1. Run the following command to change the file ownership so that the user named *nginx* can read them\. 

   ```
   sudo chown nginx /etc/pki/nginx/server.crt /etc/pki/nginx/private/server.key
   ```

1. Run the following command to back up the `/etc/nginx/nginx.conf` file\.

   ```
   sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup
   ```

1. Use a text editor to edit the `/etc/nginx/nginx.conf` file\. At the top of the file, add the following command: 

   ```
   ssl_engine cloudhsm;
   ```

   Then uncomment the TLS section of the file so that it looks like the following:

   ```
   # Settings for a TLS enabled server.
   
       server {
           listen       443 ssl http2 default_server;
           listen       [::]:443 ssl http2 default_server;
           server_name  _;
           root         /usr/share/nginx/html;
   
           ssl_certificate "/etc/pki/nginx/server.crt";
           ssl_certificate_key "/etc/pki/nginx/private/server.key";
           # It is *strongly* recommended to generate unique DH parameters
           # Generate them with: openssl dhparam -out /etc/pki/nginx/dhparams.pem 2048
           #ssl_dhparam "/etc/pki/nginx/dhparams.pem";
           ssl_session_cache shared:SSL:1m;
           ssl_session_timeout  10m;
           ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
           ssl_ciphers HIGH:SEED:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!RSAPSK:!aDH:!aECDH:!EDH-DSS-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA:!SRP;
           ssl_prefer_server_ciphers on;
   
           # Load configuration files for the default server block.
           include /etc/nginx/default.d/*.conf;
   
           location / {
           }
   
           error_page 404 /404.html;
               location = /40x.html {
           }
   
           error_page 500 502 503 504 /50x.html;
               location = /50x.html {
           }
       }
   ```

   Save the file\. This requires Linux root permissions\.

1. Back up the `systemd` configuration file, and then set the `EnvironmentFile` path\.

------
#### [ Amazon Linux ]

   No action required\.

------
#### [ Amazon Linux 2 ]

   1.  Back up the `nginx.service` file\. 

      ```
      $ sudo cp /lib/systemd/system/nginx.service /lib/systemd/system/nginx.service.backup
      ```

   1.  Open the `/lib/systemd/system/nginx.service` file in a text editor, and then under the \[Service\] section, add the following path: 

      ```
      EnvironmentFile=/etc/sysconfig/nginx
      ```

------
#### [ CentOS 6 ]

   No action required\.

------
#### [ CentOS 7 ]

   No action required\.

------
#### [ RHEL 6 ]

   No action required\.

------
#### [ RHEL 7 ]

   No action required\.

------
#### [ Ubuntu 16\.04 ]

   1.  Back up the `nginx.service` file\. 

      ```
      $ sudo cp /lib/systemd/system/nginx.service /lib/systemd/system/nginx.service.backup
      ```

   1.  Open the `/lib/systemd/system/nginx.service` file in a text editor, and then under the \[Service\] section, add the following path: 

      ```
      EnvironmentFile=/etc/sysconfig/nginx
      ```

------

1.  Check if the `/etc/sysconfig/nginx` file exists, and then do one of the following: 
   + If the file exists, back up the file by running the following command:

     ```
     $ sudo cp /etc/sysconfig/nginx /etc/sysconfig/nginx.backup
     ```
   +  If the file doesn't exist, open a text editor, and then create a file named `nginx` in the `/etc/sysconfig/` folder\. 
**Tip**  
There is no need to back up the newly created file\.

1.  Open the `/etc/sysconfig/nginx` file in a text editor, and then add the Cryptography User \(CU\) credentials: 

   ```
   $ n3fips_password=<CU user name>:<password>
   ```

    Replace the *<CU user name>* and *<password>* with the cryptography user credentials\.

    Save the file\. This requires Linux root permissions\. 

1. Start the NGINX web server\.

------
#### [ Amazon Linux ]

   ```
   $ sudo service nginx start
   ```

------
#### [ Amazon Linux 2 ]

   ```
   $ sudo systemctl start nginx
   ```

------
#### [ CentOS 6 ]

   No action required\.

------
#### [ CentOS 7 ]

   No action required\.

------
#### [ RHEL 6 ]

   No action required\.

------
#### [ RHEL 7 ]

   No action required\.

------
#### [ Ubuntu 16\.04 ]

   ```
   $ sudo systemctl start nginx
   ```

------

1. Configure your server to start NGINX when the server starts, if needed\. 

------
#### [ Amazon Linux ]

   ```
   $ sudo chkconfig nginx on
   ```

------
#### [ Amazon Linux 2 ]

   ```
   $ sudo systemctl enable nginx
   ```

------
#### [ CentOS 6 ]

   No action required\.

------
#### [ CentOS 7 ]

   No action required\.

------
#### [ RHEL 6 ]

   No action required\.

------
#### [ RHEL 7 ]

   No action required\.

------
#### [ Ubuntu 16\.04 ]

   ```
   $ sudo systemctl enable nginx
   ```

------

After you update your web server configuration, go to [Step 4: Enable HTTPS Traffic and Verify the Certificate](ssl-offload-enable-traffic-and-verify-certificate.md)\.<a name="update-web-server-config-apache"></a>

**To update the web server configuration for Apache**

1. Connect to your Amazon EC2 client instance\.

1. Run the following command to make a backup copy of the default certificate\.

   ```
   sudo cp /etc/pki/tls/certs/localhost.crt /etc/pki/tls/certs/localhost.crt.backup
   ```

1. Run the following command to make a backup copy of the default private key\.

   ```
   sudo cp /etc/pki/tls/private/localhost.key /etc/pki/tls/private/localhost.key.backup
   ```

1. Run the following command to copy your web server certificate to the required location\. Replace *<web\_server\.crt>* with the name of your web server certificate\. 

   ```
   sudo cp <web_server.crt> /etc/pki/tls/certs/localhost.crt
   ```

1. Run the following command to copy your fake PEM private key to the required location\. Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

   ```
   sudo cp <web_server_fake_PEM.key> /etc/pki/tls/private/localhost.key
   ```

1. Run the following command to change the ownership of these files so that the user named *apache* can read them\. 

   ```
   sudo chown apache /etc/pki/tls/certs/localhost.crt /etc/pki/tls/private/localhost.key
   ```

1. Run the following command to make a backup copy of the file named `/etc/httpd/conf.d/ssl.conf`\.

   ```
   sudo cp /etc/httpd/conf.d/ssl.conf /etc/httpd/conf.d/ssl.conf.backup
   ```

1. Use a text editor to edit the file named `/etc/httpd/conf.d/ssl.conf`\. Replace the line that starts with `SSLCryptoDevice` so that it looks like the following: 

   ```
   SSLCryptoDevice cloudhsm
   ```

   Save the file\. This requires Linux root permissions\.

1. Run the following command to back up the `/etc/apache2/envvars` file\.

   ```
   sudo cp /etc/apache2/envvars /etc/apache2/envvars.backup
   ```

1. Use a text editor to edit the `/etc/apache2/envvars` file\. Add the following command, specifying the user name and password of the crypto user \(CU\)\. Replace *<CU user name>* with the name of the crypto user\. Replace *<password>* with the CU password\.

   ```
   export n3fips_password=<CU user name>:<password>
   ```

   Save the file\. This requires Linux root permissions\.

1. Run the following command to start the Apache HTTP Server\.

   ```
   sudo service httpd start
   ```

1. Run the following command to configure your server to start Apache when the server starts\.

   ```
   sudo chkconfig httpd on
   ```

After you update your web server configuration, go to [Step 4: Enable HTTPS Traffic and Verify the Certificate](ssl-offload-enable-traffic-and-verify-certificate.md)\.