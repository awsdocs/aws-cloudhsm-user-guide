# Step 3: Configure the web server<a name="ssl-offload-configure-web-server"></a>

Update your web server software's configuration to use the HTTPS certificate and corresponding fake PEM private key that you created in the [previous step](ssl-offload-import-or-generate-private-key-and-certificate.md)\. Remember to backup your existing certificates and keys before you start\. This will finish setting up your Linux web server software for SSL/TLS offload with AWS CloudHSM\.

Complete the steps from one of the following sections\. 

**Topics**
+ [Configure NGINX web server](#ssl-offload-nginx)
+ [Configure Apache web server](#ssl-offload-apache)

## Configure NGINX web server<a name="ssl-offload-nginx"></a>

Use this section to configure NGINX on supported platforms\.<a name="update-web-server-config-nginx"></a>

**To update the web server configuration for NGINX**

1. Connect to your client instance\.

1. Run the following command to create the required directories for the web server certificate and the fake PEM private key\. 

   ```
   $ sudo mkdir -p /etc/pki/nginx/private
   ```

1. Run the following command to copy your web server certificate to the required location\. Replace *<web\_server\.crt>* with the name of your web server certificate\.

   ```
   $ sudo cp <web_server.crt> /etc/pki/nginx/server.crt
   ```

1. Run the following command to copy your fake PEM private key to the required location\. Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

   ```
   $ sudo cp <web_server_fake_PEM.key> /etc/pki/nginx/private/server.key
   ```

1. Run the following command to change the file ownership so that the user named *nginx* can read them\. 

   ```
   $ sudo chown nginx /etc/pki/nginx/server.crt /etc/pki/nginx/private/server.key
   ```

1. Run the following command to back up the `/etc/nginx/nginx.conf` file\.

   ```
   $ sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup
   ```

1. Update the NGINX configuration\.
**Note**  
Each cluster can support a maximum of 1000 NGINX worker processes across all NGINX web servers\.

------
#### [ Amazon Linux ]

   Use a text editor to edit the `/etc/nginx/nginx.conf` file\. This requires Linux root permissions\. At the top of the file, add the following lines: 
   + If using Client SDK 3

     ```
     ssl_engine cloudhsm;
     env n3fips_password;
     ```
   + If using Client SDK 5

     ```
     ssl_engine cloudhsm;
     env CLOUDHSM_PIN;
     ```

   Then add the following to the TLS section of the file:

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
       ssl_protocols TLSv1.2;
       ssl_ciphers "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA";
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

------
#### [ Amazon Linux 2 ]

   Use a text editor to edit the `/etc/nginx/nginx.conf` file\. This requires Linux root permissions\. At the top of the file, add the following lines: 
   + If using Client SDK 3

     ```
     ssl_engine cloudhsm;
     env n3fips_password;
     ```
   + If using Client SDK 5

     ```
     ssl_engine cloudhsm;
     env CLOUDHSM_PIN;
     ```

   Then add the following to the TLS section of the file:

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
       ssl_protocols TLSv1.2;
       ssl_ciphers "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA";
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

------
#### [ CentOS 7 ]

   Use a text editor to edit the `/etc/nginx/nginx.conf` file\. This requires Linux root permissions\. At the top of the file, add the following lines: 
   + If using Client SDK 3

     ```
     ssl_engine cloudhsm;
     env n3fips_password;
     ```
   + If using Client SDK 5

     ```
     ssl_engine cloudhsm;
     env CLOUDHSM_PIN;
     ```

   Then add the following to the TLS section of the file:

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
       ssl_protocols TLSv1.2;
       ssl_ciphers "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA";
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

------
#### [ CentOS 8 ]

   Use a text editor to edit the `/etc/nginx/nginx.conf` file\. This requires Linux root permissions\. At the top of the file, add the following lines: 

   ```
   ssl_engine cloudhsm;
   env CLOUDHSM_PIN;
   ```

   Then add the following to the TLS section of the file:

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
       ssl_protocols TLSv1.2 TLSv1.3;
       ssl_ciphers "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA";
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

------
#### [ Red Hat 7 ]

   Use a text editor to edit the `/etc/nginx/nginx.conf` file\. This requires Linux root permissions\. At the top of the file, add the following lines: 
   + If using Client SDK 3

     ```
     ssl_engine cloudhsm;
     env n3fips_password;
     ```
   + If using Client SDK 5

     ```
     ssl_engine cloudhsm;
     env CLOUDHSM_PIN;
     ```

   Then add the following to the TLS section of the file:

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
       ssl_protocols TLSv1.2;
       ssl_ciphers "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA";
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

------
#### [ Red Hat 8 ]

   Use a text editor to edit the `/etc/nginx/nginx.conf` file\. This requires Linux root permissions\. At the top of the file, add the following lines: 

   ```
   ssl_engine cloudhsm;
   env CLOUDHSM_PIN;
   ```

   Then add the following to the TLS section of the file:

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
       ssl_protocols TLSv1.2 TLSv1.3;
       ssl_ciphers "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA";
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

------
#### [ Ubuntu 16\.04 LTS ]

   Use a text editor to edit the `/etc/nginx/nginx.conf` file\. This requires Linux root permissions\. At the top of the file, add the following lines: 

   ```
   ssl_engine cloudhsm;
   env n3fips_password;
   ```

   Then add the following to the TLS section of the file:

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
       ssl_protocols TLSv1.2;
       ssl_ciphers "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA";
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

------
#### [ Ubuntu 18\.04 LTS ]

   Use a text editor to edit the `/etc/nginx/nginx.conf` file\. This requires Linux root permissions\. At the top of the file, add the following lines: 

   ```
   ssl_engine cloudhsm;
   env CLOUDHSM_PIN;
   ```

   Then add the following to the TLS section of the file:

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
       ssl_protocols TLSv1.2 TLSv1.3;
       ssl_ciphers "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA";
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

------

   Save the file\.

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
#### [ CentOS 7 ]

   No action required\.

------
#### [ CentOS 8 ]

   1.  Back up the `nginx.service` file\. 

      ```
      $ sudo cp /lib/systemd/system/nginx.service /lib/systemd/system/nginx.service.backup
      ```

   1.  Open the `/lib/systemd/system/nginx.service` file in a text editor, and then under the \[Service\] section, add the following path: 

      ```
      EnvironmentFile=/etc/sysconfig/nginx
      ```

------
#### [ Red Hat 7 ]

   No action required\.

------
#### [ Red Hat 8 ]

   1.  Back up the `nginx.service` file\. 

      ```
      $ sudo cp /lib/systemd/system/nginx.service /lib/systemd/system/nginx.service.backup
      ```

   1.  Open the `/lib/systemd/system/nginx.service` file in a text editor, and then under the \[Service\] section, add the following path: 

      ```
      EnvironmentFile=/etc/sysconfig/nginx
      ```

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
#### [ Ubuntu 18\.04 ]

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

1. Configure the NGINX environment\.
**Note**  
Client SDK 5 introduces the `CLOUDHSM_PIN` environment variable for storing the credentials of the CU\. In Client SDK 3 you stored the CU credentials in the `n3fips_password` environment variable\. Client SDK 5 supports both environment variables, but we recommend using `CLOUDHSM_PIN`\.

------
#### [ Amazon Linux ]

   Open the `/etc/sysconfig/nginx` file in a text editor\. This requires Linux root permissions\. Add the Cryptography User \(CU\) credentials:
   + If using Client SDK 3

     ```
     n3fips_password=<CU user name>:<password>
     ```
   + If using Client SDK 5

     ```
     CLOUDHSM_PIN=<CU user name>:<password>
     ```

   Replace *<CU user name>* and *<password>* with the CU credentials\. 

    Save the file\.

------
#### [ Amazon Linux 2 ]

   Open the `/etc/sysconfig/nginx` file in a text editor\. This requires Linux root permissions\. Add the Cryptography User \(CU\) credentials:
   + If using Client SDK 3

     ```
     n3fips_password=<CU user name>:<password>
     ```
   + If using Client SDK 5

     ```
     CLOUDHSM_PIN=<CU user name>:<password>
     ```

   Replace *<CU user name>* and *<password>* with the CU credentials\. 

    Save the file\.

------
#### [ CentOS 7 ]

   Open the `/etc/sysconfig/nginx` file in a text editor\. This requires Linux root permissions\. Add the Cryptography User \(CU\) credentials:
   + If using Client SDK 3

     ```
     n3fips_password=<CU user name>:<password>
     ```
   + If using Client SDK 5

     ```
     CLOUDHSM_PIN=<CU user name>:<password>
     ```

   Replace *<CU user name>* and *<password>* with the CU credentials\. 

    Save the file\.

------
#### [ CentOS 8 ]

   Open the `/etc/sysconfig/nginx` file in a text editor\. This requires Linux root permissions\. Add the Cryptography User \(CU\) credentials:

   ```
   CLOUDHSM_PIN=<CU user name>:<password>
   ```

   Replace *<CU user name>* and *<password>* with the CU credentials\. 

    Save the file\.

------
#### [ Red Hat 7 ]

   Open the `/etc/sysconfig/nginx` file in a text editor\. This requires Linux root permissions\. Add the Cryptography User \(CU\) credentials:
   + If using Client SDK 3

     ```
     n3fips_password=<CU user name>:<password>
     ```
   + If using Client SDK 5

     ```
     CLOUDHSM_PIN=<CU user name>:<password>
     ```

   Replace *<CU user name>* and *<password>* with the CU credentials\. 

    Save the file\.

------
#### [ Red Hat 8 ]

   Open the `/etc/sysconfig/nginx` file in a text editor\. This requires Linux root permissions\. Add the Cryptography User \(CU\) credentials:

   ```
   CLOUDHSM_PIN=<CU user name>:<password>
   ```

   Replace *<CU user name>* and *<password>* with the CU credentials\. 

    Save the file\.

------
#### [ Ubuntu 16\.04 LTS ]

   Open the `/etc/sysconfig/nginx` file in a text editor\. This requires Linux root permissions\. Add the Cryptography User \(CU\) credentials:

   ```
   n3fips_password=<CU user name>:<password>
   ```

   Replace *<CU user name>* and *<password>* with the CU credentials\. 

    Save the file\.

------
#### [ Ubuntu 18\.04 LTS ]

   Open the `/etc/sysconfig/nginx` file in a text editor\. This requires Linux root permissions\. Add the Cryptography User \(CU\) credentials:

   ```
   CLOUDHSM_PIN=<CU user name>:<password>
   ```

   Replace *<CU user name>* and *<password>* with the CU credentials\. 

    Save the file\.

------

1. Start the NGINX web server\.

------
#### [ Amazon Linux ]

   Open the `/etc/sysconfig/nginx` file in a text editor\. This requires Linux root permissions\. Add the Cryptography User \(CU\) credentials:

   ```
   $ sudo service nginx start
   ```

------
#### [ Amazon Linux 2 ]

   Stop any running NGINX process

   ```
   $ sudo systemctl stop nginx
   ```

   Reload the `systemd` configuration to pick up the latest changes

   ```
   $ sudo systemctl daemon-reload
   ```

   Start the NGINX process

   ```
   $ sudo systemctl start nginx
   ```

------
#### [ CentOS 7 ]

   Stop any running NGINX process

   ```
   $ sudo systemctl stop nginx
   ```

   Reload the `systemd` configuration to pick up the latest changes

   ```
   $ sudo systemctl daemon-reload
   ```

   Start the NGINX process

   ```
   $ sudo systemctl start nginx
   ```

------
#### [ CentOS 8 ]

   Stop any running NGINX process

   ```
   $ sudo systemctl stop nginx
   ```

   Reload the `systemd` configuration to pick up the latest changes

   ```
   $ sudo systemctl daemon-reload
   ```

   Start the NGINX process

   ```
   $ sudo systemctl start nginx
   ```

------
#### [ Red Hat 7 ]

   Stop any running NGINX process

   ```
   $ sudo systemctl stop nginx
   ```

   Reload the `systemd` configuration to pick up the latest changes

   ```
   $ sudo systemctl daemon-reload
   ```

   Start the NGINX process

   ```
   $ sudo systemctl start nginx
   ```

------
#### [ Red Hat 8 ]

   Stop any running NGINX process

   ```
   $ sudo systemctl stop nginx
   ```

   Reload the `systemd` configuration to pick up the latest changes

   ```
   $ sudo systemctl daemon-reload
   ```

   Start the NGINX process

   ```
   $ sudo systemctl start nginx
   ```

------
#### [ Ubuntu 16\.04 ]

   Stop any running NGINX process

   ```
   $ sudo systemctl stop nginx
   ```

   Reload the `systemd` configuration to pick up the latest changes

   ```
   $ sudo systemctl daemon-reload
   ```

   Start the NGINX process

   ```
   $ sudo systemctl start nginx
   ```

------
#### [ Ubuntu 18\.04 ]

   Stop any running NGINX process

   ```
   $ sudo systemctl stop nginx
   ```

   Reload the `systemd` configuration to pick up the latest changes

   ```
   $ sudo systemctl daemon-reload
   ```

   Start the NGINX process

   ```
   $ sudo systemctl start nginx
   ```

------

1. \(Optional\) Configure your platform to start NGINX at start\-up\.

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
#### [ CentOS 7 ]

   No action required\.

------
#### [ CentOS 8 ]

   ```
   $ sudo systemctl enable nginx
   ```

------
#### [ Red Hat 7 ]

   No action required\.

------
#### [ Red Hat 8 ]

   ```
   $ sudo systemctl enable nginx
   ```

------
#### [ Ubuntu 16\.04 ]

   ```
   $ sudo systemctl enable nginx
   ```

------
#### [ Ubuntu 18\.04 ]

   ```
   $ sudo systemctl enable nginx
   ```

------

After you update your web server configuration, go to [Step 4: Enable HTTPS traffic and verify the certificate](ssl-offload-enable-traffic-and-verify-certificate.md)\.

## Configure Apache web server<a name="ssl-offload-apache"></a>

 Use this section to configure Apache on supported platforms\. <a name="update-web-server-config-apache"></a>

**To update the web server configuration for Apache**

1. Connect to your Amazon EC2 client instance\.

1. Define default locations for certificates and private keys for your platform\.

------
#### [ Amazon Linux ]

   In the `/etc/httpd/conf.d/ssl.conf` file, ensure these values exist:

   ```
   SSLCertificateFile      /etc/pki/tls/certs/localhost.crt
   SSLCertificateKeyFile   /etc/pki/tls/private/localhost.key
   ```

------
#### [ Amazon Linux 2 ]

   In the `/etc/httpd/conf.d/ssl.conf` file, ensure these values exist:

   ```
   SSLCertificateFile      /etc/pki/tls/certs/localhost.crt
   SSLCertificateKeyFile   /etc/pki/tls/private/localhost.key
   ```

------
#### [ CentOS 7 ]

   In the `/etc/httpd/conf.d/ssl.conf` file, ensure these values exist:

   ```
   SSLCertificateFile      /etc/pki/tls/certs/localhost.crt
   SSLCertificateKeyFile   /etc/pki/tls/private/localhost.key
   ```

------
#### [ CentOS 8 ]

   In the `/etc/httpd/conf.d/ssl.conf` file, ensure these values exist:

   ```
   SSLCertificateFile      /etc/pki/tls/certs/localhost.crt
   SSLCertificateKeyFile   /etc/pki/tls/private/localhost.key
   ```

------
#### [ Red Hat 7 ]

   In the `/etc/httpd/conf.d/ssl.conf` file, ensure these values exist:

   ```
   SSLCertificateFile      /etc/pki/tls/certs/localhost.crt
   SSLCertificateKeyFile   /etc/pki/tls/private/localhost.key
   ```

------
#### [ Red Hat 8 ]

   In the `/etc/httpd/conf.d/ssl.conf` file, ensure these values exist:

   ```
   SSLCertificateFile      /etc/pki/tls/certs/localhost.crt
   SSLCertificateKeyFile   /etc/pki/tls/private/localhost.key
   ```

------
#### [ Ubuntu 16\.04 LTS ]

   In the `/etc/apache2/sites-available/default-ssl.conf` file, ensure these values exist:

   ```
   SSLCertificateFile      /etc/ssl/certs/localhost.crt
   SSLCertificateKeyFile   /etc/ssl/private/localhost.key
   ```

------
#### [ Ubuntu 18\.04 LTS ]

   In the `/etc/apache2/sites-available/default-ssl.conf` file, ensure these values exist:

   ```
   SSLCertificateFile      /etc/ssl/certs/localhost.crt
   SSLCertificateKeyFile   /etc/ssl/private/localhost.key
   ```

------

1. Copy your web server certificate to the required location for your platform\.

------
#### [ Amazon Linux ]

   ```
   $ sudo cp <web_server.crt> /etc/pki/tls/certs/localhost.crt
   ```

   Replace *<web\_server\.crt>* with the name of your web server certificate\. 

------
#### [ Amazon Linux 2 ]

   ```
   $ sudo cp <web_server.crt> /etc/pki/tls/certs/localhost.crt
   ```

   Replace *<web\_server\.crt>* with the name of your web server certificate\. 

------
#### [ CentOS 7 ]

   ```
   $ sudo cp <web_server.crt> /etc/pki/tls/certs/localhost.crt
   ```

   Replace *<web\_server\.crt>* with the name of your web server certificate\. 

------
#### [ CentOS 8 ]

   ```
   $ sudo cp <web_server.crt> /etc/pki/tls/certs/localhost.crt
   ```

   Replace *<web\_server\.crt>* with the name of your web server certificate\. 

------
#### [ Red Hat 7 ]

   ```
   $ sudo cp <web_server.crt> /etc/pki/tls/certs/localhost.crt
   ```

   Replace *<web\_server\.crt>* with the name of your web server certificate\. 

------
#### [ Red Hat 8 ]

   ```
   $ sudo cp <web_server.crt> /etc/pki/tls/certs/localhost.crt
   ```

   Replace *<web\_server\.crt>* with the name of your web server certificate\. 

------
#### [ Ubuntu 16\.04 LTS ]

   ```
   $ sudo cp <web_server.crt> /etc/ssl/certs/localhost.crt
   ```

   Replace *<web\_server\.crt>* with the name of your web server certificate\. 

------
#### [ Ubuntu 18\.04 LTS ]

   ```
   $ sudo cp <web_server.crt> /etc/ssl/certs/localhost.crt
   ```

   Replace *<web\_server\.crt>* with the name of your web server certificate\. 

------

1. Copy your fake PEM private key to the required location for your platform\.

------
#### [ Amazon Linux ]

   ```
   $ sudo cp <web_server_fake_PEM.key> /etc/pki/tls/private/localhost.key
   ```

   Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

------
#### [ Amazon Linux 2 ]

   ```
   $ sudo cp <web_server_fake_PEM.key> /etc/pki/tls/private/localhost.key
   ```

   Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

------
#### [ CentOS 7 ]

   ```
   $ sudo cp <web_server_fake_PEM.key> /etc/pki/tls/private/localhost.key
   ```

   Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

------
#### [ CentOS 8 ]

   ```
   $ sudo cp <web_server_fake_PEM.key> /etc/pki/tls/private/localhost.key
   ```

   Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

------
#### [ Red Hat 7 ]

   ```
   $ sudo cp <web_server_fake_PEM.key> /etc/pki/tls/private/localhost.key
   ```

   Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

------
#### [ Red Hat 8 ]

   ```
   $ sudo cp <web_server_fake_PEM.key> /etc/pki/tls/private/localhost.key
   ```

   Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

------
#### [ Ubuntu 16\.04 LTS ]

   ```
   $ sudo cp <web_server_fake_PEM.key> /etc/ssl/private/localhost.key
   ```

   Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

------
#### [ Ubuntu 18\.04 LTS ]

   ```
   $ sudo cp <web_server_fake_PEM.key> /etc/ssl/private/localhost.key
   ```

   Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

------

1. Change ownership of these files if required by your platform\.

------
#### [ Amazon Linux ]

   ```
   $ sudo chown apache /etc/pki/tls/certs/localhost.crt /etc/pki/tls/private/localhost.key
   ```

   Provides read permission to the user named *apache*\.

------
#### [ Amazon Linux 2 ]

   ```
   $ sudo chown apache /etc/pki/tls/certs/localhost.crt /etc/pki/tls/private/localhost.key
   ```

   Provides read permission to the user named *apache*\.

------
#### [ CentOS 7 ]

   ```
   $ sudo chown apache /etc/pki/tls/certs/localhost.crt /etc/pki/tls/private/localhost.key
   ```

   Provides read permission to the user named *apache*\.

------
#### [ CentOS 8 ]

   ```
   $ sudo chown apache /etc/pki/tls/certs/localhost.crt /etc/pki/tls/private/localhost.key
   ```

   Provides read permission to the user named *apache*\.

------
#### [ Red Hat 7 ]

   ```
   $ sudo chown apache /etc/pki/tls/certs/localhost.crt /etc/pki/tls/private/localhost.key
   ```

   Provides read permission to the user named *apache*\.

------
#### [ Red Hat 8 ]

   ```
   $ sudo chown apache /etc/pki/tls/certs/localhost.crt /etc/pki/tls/private/localhost.key
   ```

   Provides read permission to the user named *apache*\.

------
#### [ Ubuntu 16\.04 LTS ]

   No action required\.

------
#### [ Ubuntu 18\.04 LTS ]

   No action required\.

------

1. Configure Apache directives for your platform\.

------
#### [ Amazon Linux ]

   Locate the SSL file for this platform:

   ```
   /etc/httpd/conf.d/ssl.conf
   ```

   This file contains Apache directives which define how your server should run\. Directives appear on the left, followed by a value\. Use a text editor to edit this file\. This requires Linux root permissions\.

   Update or enter the following directives with these values:

   ```
   SSLCryptoDevice cloudhsm
   SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA
   ```

   Save the file\.

------
#### [ Amazon Linux 2 ]

   Locate the SSL file for this platform:

   ```
   /etc/httpd/conf.d/ssl.conf
   ```

   This file contains Apache directives which define how your server should run\. Directives appear on the left, followed by a value\. Use a text editor to edit this file\. This requires Linux root permissions\.

   Update or enter the following directives with these values:

   ```
   SSLCryptoDevice cloudhsm
   SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA
   ```

   Save the file\.

------
#### [ CentOS 7 ]

   Locate the SSL file for this platform:

   ```
   /etc/httpd/conf.d/ssl.conf
   ```

   This file contains Apache directives which define how your server should run\. Directives appear on the left, followed by a value\. Use a text editor to edit this file\. This requires Linux root permissions\.

   Update or enter the following directives with these values:

   ```
   SSLCryptoDevice cloudhsm
   SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA
   ```

   Save the file\.

------
#### [ CentOS 8 ]

   Locate the SSL file for this platform:

   ```
   /etc/httpd/conf.d/ssl.conf
   ```

   This file contains Apache directives which define how your server should run\. Directives appear on the left, followed by a value\. Use a text editor to edit this file\. This requires Linux root permissions\.

   Update or enter the following directives with these values:

   ```
   SSLCryptoDevice cloudhsm
   SSLProtocol TLSv1.2 TLSv1.3
   SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA
   SSLProxyCipherSuite HIGH:!aNULL
   ```

   Save the file\.

------
#### [ Red Hat 7 ]

   Locate the SSL file for this platform:

   ```
   /etc/httpd/conf.d/ssl.conf
   ```

   This file contains Apache directives which define how your server should run\. Directives appear on the left, followed by a value\. Use a text editor to edit this file\. This requires Linux root permissions\.

   Update or enter the following directives with these values:

   ```
   SSLCryptoDevice cloudhsm
   SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA
   ```

   Save the file\.

------
#### [ Red Hat 8 ]

   Locate the SSL file for this platform:

   ```
   /etc/httpd/conf.d/ssl.conf
   ```

   This file contains Apache directives which define how your server should run\. Directives appear on the left, followed by a value\. Use a text editor to edit this file\. This requires Linux root permissions\.

   Update or enter the following directives with these values:

   ```
   SSLCryptoDevice cloudhsm
   SSLProtocol TLSv1.2 TLSv1.3
   SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA
   SSLProxyCipherSuite HIGH:!aNULL
   ```

   Save the file\.

------
#### [ Ubuntu 16\.04 LTS ]

   Locate the SSL file for this platform:

   ```
   /etc/apache2/mods-available/ssl.conf
   ```

   This file contains Apache directives which define how your server should run\. Directives appear on the left, followed by a value\. Use a text editor to edit this file\. This requires Linux root permissions\.

   Update or enter the following directives with these values:

   ```
   SSLCryptoDevice cloudhsm
   SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA
   ```

   Save the file\.

   Enable the SSL module and default SSL site configuration:

   ```
   $ sudo a2enmod ssl
   $ sudo a2ensite default-ssl
   ```

------
#### [ Ubuntu 18\.04 LTS ]

   Locate the SSL file for this platform:

   ```
   /etc/apache2/mods-available/ssl.conf
   ```

   This file contains Apache directives which define how your server should run\. Directives appear on the left, followed by a value\. Use a text editor to edit this file\. This requires Linux root permissions\.

   Update or enter the following directives with these values:

   ```
   SSLCryptoDevice cloudhsm
   SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA
   SSLProtocol TLSv1.2 TLSv1.3
   ```

   Save the file\.

   Enable the SSL module and default SSL site configuration:

   ```
   $ sudo a2enmod ssl
   $ sudo a2ensite default-ssl
   ```

------

1. Configure an environment\-values file for your platform\.

------
#### [ Amazon Linux ]

   No action required\. Environment values go in `/etc/sysconfig/httpd`

------
#### [ Amazon Linux 2 ]

    Open the httpd service file: 

   ```
   /lib/systemd/system/httpd.service
   ```

    Under the `[Service]` section, add the following: 

   ```
   EnvironmentFile=/etc/sysconfig/httpd
   ```

------
#### [ CentOS 7 ]

    Open the httpd service file: 

   ```
   /lib/systemd/system/httpd.service
   ```

    Under the `[Service]` section, add the following: 

   ```
   EnvironmentFile=/etc/sysconfig/httpd
   ```

------
#### [ CentOS 8 ]

    Open the httpd service file: 

   ```
   /lib/systemd/system/httpd.service
   ```

    Under the `[Service]` section, add the following: 

   ```
   EnvironmentFile=/etc/sysconfig/httpd
   ```

------
#### [ Red Hat 7 ]

    Open the httpd service file: 

   ```
   /lib/systemd/system/httpd.service
   ```

    Under the `[Service]` section, add the following: 

   ```
   EnvironmentFile=/etc/sysconfig/httpd
   ```

------
#### [ Red Hat 8 ]

    Open the httpd service file: 

   ```
   /lib/systemd/system/httpd.service
   ```

    Under the `[Service]` section, add the following: 

   ```
   EnvironmentFile=/etc/sysconfig/httpd
   ```

------
#### [ Ubuntu 16\.04 LTS ]

   No action required\. Environment values go in `/etc/sysconfig/httpd`

------
#### [ Ubuntu 18\.04 LTS ]

   No action required\. Environment values go in `/etc/sysconfig/httpd`

------

1. In the file that stores environment variables for your platform, set an environment variable that contains the credentials of the cryptographic user \(CU\):

------
#### [ Amazon Linux ]

   Use a text editor to edit the `/etc/sysconfig/httpd`\.
   + If using Client SDK 3

     ```
     n3fips_password=<CU user name>:<password>
     ```
   + If using Client SDK 5

     ```
     CLOUDHSM_PIN=<CU user name>:<password>
     ```

   Replace *<CU user name>* and *<password>* with the CU credentials\.

------
#### [ Amazon Linux 2 ]

   Use a text editor to edit the `/etc/sysconfig/httpd`\.
   + If using Client SDK 3

     ```
     n3fips_password=<CU user name>:<password>
     ```
   + If using Client SDK 5

     ```
     CLOUDHSM_PIN=<CU user name>:<password>
     ```

   Replace *<CU user name>* and *<password>* with the CU credentials\.

------
#### [ CentOS 7 ]

   Use a text editor to edit the `/etc/sysconfig/httpd`\.
   + If using Client SDK 3

     ```
     n3fips_password=<CU user name>:<password>
     ```
   + If using Client SDK 5

     ```
     CLOUDHSM_PIN=<CU user name>:<password>
     ```

   Replace *<CU user name>* and *<password>* with the CU credentials\.

------
#### [ CentOS 8 ]

   Use a text editor to edit the `/etc/sysconfig/httpd`\.

   ```
   CLOUDHSM_PIN=<CU user name>:<password>
   ```

   Replace *<CU user name>* and *<password>* with the CU credentials\.

------
#### [ Red Hat 7 ]

   Use a text editor to edit the `/etc/sysconfig/httpd`\.
   + If using Client SDK 3

     ```
     n3fips_password=<CU user name>:<password>
     ```
   + If using Client SDK 5

     ```
     CLOUDHSM_PIN=<CU user name>:<password>
     ```

   Replace *<CU user name>* and *<password>* with the CU credentials\.

------
#### [ Red Hat 8 ]

   Use a text editor to edit the `/etc/sysconfig/httpd`\.

   ```
   CLOUDHSM_PIN=<CU user name>:<password>
   ```

   Replace *<CU user name>* and *<password>* with the CU credentials\.

**Note**  
Client SDK 5 introduces the `CLOUDHSM_PIN` environment variable for storing the credentials of the CU\. In Client SDK 3 you stored the CU credentials in the `n3fips_password` environment variable\. Client SDK 5 supports both environment variables, but we recommend using `CLOUDHSM_PIN`\.

------
#### [ Ubuntu 16\.04 LTS ]

   Use a text editor to edit the `/etc/apache2/envvars`\.

   ```
   export n3fips_password=<CU user name>:<password>
   ```

   Replace *<CU user name>* and *<password>* with the CU credentials\.

------
#### [ Ubuntu 18\.04 LTS ]

   Use a text editor to edit the `/etc/apache2/envvars`\.

   ```
   export CLOUDHSM_PIN=<CU user name>:<password>
   ```

   Replace *<CU user name>* and *<password>* with the CU credentials\.

**Note**  
Client SDK 5 introduces the `CLOUDHSM_PIN` environment variable for storing the credentials of the CU\. In Client SDK 3 you stored the CU credentials in the `n3fips_password` environment variable\. Client SDK 5 supports both environment variables, but we recommend using `CLOUDHSM_PIN`\.

------

1. Start the Apache web server\.

------
#### [ Amazon Linux ]

   ```
   $ sudo systemctl daemon-reload
   $ sudo service httpd start
   ```

------
#### [ Amazon Linux 2 ]

   ```
   $ sudo systemctl daemon-reload
   $ sudo service httpd start
   ```

------
#### [ CentOS 7 ]

   ```
   $ sudo systemctl daemon-reload
   $ sudo service httpd start
   ```

------
#### [ CentOS 8 ]

   ```
   $ sudo systemctl daemon-reload
   $ sudo service httpd start
   ```

------
#### [ Red Hat 7 ]

   ```
   $ sudo systemctl daemon-reload
   $ sudo service httpd start
   ```

------
#### [ Red Hat 8 ]

   ```
   $ sudo systemctl daemon-reload
   $ sudo service httpd start
   ```

------
#### [ Ubuntu 16\.04 LTS ]

   ```
   $ sudo service apache2 start
   ```

------
#### [ Ubuntu 18\.04 LTS ]

   ```
   $ sudo service apache2 start
   ```

------

1. \(Optional\) Configure your platform to start Apache at start\-up\.

------
#### [ Amazon Linux ]

   ```
   $ sudo chkconfig httpd on
   ```

------
#### [ Amazon Linux 2 ]

   ```
   $ sudo chkconfig httpd on
   ```

------
#### [ CentOS 7 ]

   ```
   $ sudo chkconfig httpd on
   ```

------
#### [ CentOS 8 ]

   ```
   $ systemctl enable httpd
   ```

------
#### [ Red Hat 7 ]

   ```
   $ sudo chkconfig httpd on
   ```

------
#### [ Red Hat 8 ]

   ```
   $ systemctl enable httpd
   ```

------
#### [ Ubuntu 16\.04 LTS ]

   ```
   $ sudo systemctl enable apache2
   ```

------
#### [ Ubuntu 18\.04 LTS ]

   ```
   $ sudo systemctl enable apache2
   ```

------

After you update your web server configuration, go to [Step 4: Enable HTTPS traffic and verify the certificate](ssl-offload-enable-traffic-and-verify-certificate.md)\.