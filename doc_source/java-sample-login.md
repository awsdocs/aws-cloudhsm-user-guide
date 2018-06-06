# Log In To and Out Of an HSM<a name="java-sample-login"></a>


|  | 
| --- |
|   \*\* Example code only \- Not for production use \*\* This page includes example code that has not been fully tested\. It is designed for test environments\. Do not run this code in production\.  | 

This example demonstrates multiple ways for your Java application to log in to the HSMs in your cluster\. These methods use the `LoginManager` class to manage sessions in the code\. Each provides credentials in a different way\. For more details about providing credentials to the Java library, see [Providing Credentials to the Java Library](java-library-install.md#java-library-credentials)\.

The remaining samples in the [Code Samples for the AWS CloudHSM Software Library for Java](java-lib-samples.md) section use the `loginWithEnvVariables()` method to log in to an HSM\. This method fails unless you set the HSM environment variables in advance\. You can change them to use the method that you prefer\.

**Note**  
To run this example, replace *<hsm\-user\-name>* and *<password>* with a valid AWS CloudHSM user name and password\. 

```
package com.amazonaws.cloudhsm.examples;
 
import com.cavium.cfm2.CFM2Exception;
import com.cavium.cfm2.LoginManager;
 
public class LoginLogoutExample {
  public static void main(String[] z) {
    System.out.println("*********** Logging in Using Hardcoded Credentials ***********");
    loginWithExplicitCredentials();
    
    System.out.println("*********** Logging in Using System.Properties ***********"); 
    loginUsingJavaProperties();
    
    System.out.println("*********** Logging in Using Environment Variables ***********"); 
    loginWithEnvVariables();
    
    System.out.println("Logging out of Session");
    logout();
  }
  
/**
 * Method #1: Use hard-coded credentials
 *
 * Replace "<hsm-user-name>" and "<password>" with a valid user name and password.
 */
  public static void loginWithExplicitCredentials() {
    LoginManager lm = LoginManager.getInstance();
    lm.loadNative();
    try {
      lm.login("PARTITION_1", "<hsm-user-name>", "<password>");
      int appID = lm.getAppid();
      int sessionID = lm.getSessionid();
    } catch (CFM2Exception e) {
      e.printStackTrace();
    }
  }
 
  /**
   * Method #2: Use Java system properties
   *
   * Replace "<hsm-user-name>" and "<password>" with a valid user name and password.
   */
  public static void loginUsingJavaProperties() {
    System.setProperty("HSM_PARTITION","PARTITION_1"); 
    System.setProperty("HSM_USER","<hsm-user-name>"); 
    System.setProperty("HSM_PASSWORD","<password>");
    LoginManager lm = LoginManager.getInstance();
    lm.loadNative();
    try {
      lm.login();
      int appID = lm.getAppid();
      int sessionID = lm.getSessionid();
    } catch (CFM2Exception e) {
      e.printStackTrace();
    }
  }
 
  /**
   * Method #3: Use environment variables
   *
   * Before invoking the JVM, replace "<hsm-user-name>" 
   * and "<password>" with a valid user name and password.
   */
   *    export HSM_PARTITION=PARTITION_1
   *    export HSM_USER=<hsm-user-name>
   *    export HSM_PASSWORD=<password>
   */
  public static void loginWithEnvVariables() {
    LoginManager lm = LoginManager.getInstance();
    lm.loadNative();
    try {
      lm.login();
      int appID = lm.getAppid();
      int sessionID = lm.getSessionid();
    } catch (CFM2Exception e) {
      e.printStackTrace();
    }
  }
 
  public static void logout() {
    try {
      LoginManager.getInstance().logout();
    } catch (CFM2Exception e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
    }
  }
}
```