# Example Code for the AWS CloudHSM Software Library for Java<a name="java-library-sample"></a>


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\* This page includes example code that has not been fully tested\. It is designed for test environments\. Do not run this code in production\.  | 

The following Java code examples show you how to use the AWS CloudHSM software library for Java to perform basic tasks in AWS CloudHSM\.

Before running the examples, set up your environment:

+ Install and configure the AWS CloudHSM software library for Java and the AWS CloudHSM client package\.

+ Set up a valid HSM user name and password\. Crypto user \(CU\) permissions are sufficient for these tasks\. Your application uses these credentials to log in to the HSM in each example\. The examples use the `loginWithExplicitCredentials()` method to log in to an HSM, but you can use the method that you prefer\.

+ Decide how to specify the Cavium provider\.


+ [Specifying the Cavium Provider](#use-cavium-provider)
+ [Logging Into and Out of an HSM](#java-sample-login)
+ [Generating a Symmetric Key](#java-sample-symmetric-key)
+ [Encrypting and Decrypting with a Symmetric Key](#java-sample-symmetric-encrypt-decrypt)
+ [Generating an Asymmetric Key Pair](#java-sample-asymmetric-key)
+ [Encrypting and Decrypting with an Asymmetric Key Pair](#java-sample-asymmetric-encrypt-decrypt)
+ [Signing a Message](#java-sample-sign-message)
+ [Generating a Hash](#java-sample-hash)
+ [Generating an HMAC](#java-sample-hmac)
+ [Managing Keys in an HSM](#java-sample-manage-keys)

## Specifying the Cavium Provider<a name="use-cavium-provider"></a>

The examples that follow use the Cavium provider in the AWS CloudHSM client package\. To specify the Cavium provider, use either of the following techniques:

+ Create an instance of the Cavium provider and pass it to the methods that take a provider, such as this `KeyGenerator.getInstance()` method\.

  ```
  CaviumProvider cp = new CaviumProvider();
  keyGen = KeyGenerator.getInstance("AES",cp);
  ```

+ Add the Cavium provider to the `$JAVA_HOME/jre/lib/security/java.security` file\. Then use the **Cavium** string to refer to the provider\. If you do not specify a provider, Java uses the first provider in the file, but it's best to specify the provider explicitly\.

  ```
  //Add the Cavium provider to the Java provider file
  Security.addProvider(new CaviumProvider());
  //Or, add the provider to the first position in the file
  Security.insertProviderAt(new CaviumProvider(), 1);
  
  //Then, you can use "Cavium" to specify the provider.
  keyGen = KeyGenerator.getInstance("AES","Cavium");
  ```

## Logging Into and Out of an HSM<a name="java-sample-login"></a>

This example demonstrates three ways for your Java application to log in to the HSMs in your cluster\. These methods all use the `LoginManager` class to manage sessions in the code\. Each provides credentials in a different way\.

The remaining examples in this section use the `loginWithExplicitCredentials()` method to log in to an HSM, but you can change them to use the method that you prefer\.

**Note**  
To run this example, you must replace *CryptoUser* and *CUPassword123\!* with a valid AWS CloudHSM user name and password\. Also, the `loginWithEnvVariables()` method fails unless you have set the HSM environment variables in advance\.

For more details about providing credentials to the Java library, see [Providing Credentials to the Java Library](java-library-install.md#java-library-credentials)


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\*  | 

```
package com.amazonaws.cloudhsm.examples;

import com.cavium.cfm2.CFM2Exception;
import com.cavium.cfm2.LoginManager;

public class LoginLogoutExample {
    public static void main(String[] args) {
        System.out.println("Test three methods of logging into the HSMs in your cluster");
        System.out.println("*********** Logging in using hard-coded credentials ***********");
        loginWithExplicitCredentials();
        System.out.println("Logging out");
        logout();
        
        System.out.println("*********** Logging in using Java system properties ***********");    
        loginUsingJavaProperties();
        System.out.println("Logging out");
        logout();
        
        System.out.println("*********** Logging in using environment variables ************");    
        loginWithEnvVariables();
        System.out.println("Logging out");
        logout();
    }
/**
 * Method #1: Use hard-coded credentials
 *
 * Replace "CryptoUser" and "CUPassword123!" with a valid user name and password.
 */
    public static void loginWithExplicitCredentials() {
        LoginManager lm = LoginManager.getInstance();
        lm.loadNative();
        try {
            lm.login("PARTITION_1", "CryptoUser", "CUPassword123!");
            int appID = lm.getAppid();
            System.out.println("App ID = " + appID);
            int sessionID = lm.getSessionid();
            System.out.println("Session ID = " + sessionID);
        } catch (CFM2Exception e) {
            e.printStackTrace();
        }
    }
/**
 * Method #2: Use Java system properties
 *
 * Replace "CryptoUser" and "CUPassword123!" with a valid user name and password.
 */
    public static void loginUsingJavaProperties() {
        System.setProperty("HSM_PARTITION","PARTITION_1"); 
        System.setProperty("HSM_USER","CryptoUser"); 
        System.setProperty("HSM_PASSWORD","CUPassword123!");
        LoginManager lm = LoginManager.getInstance();
        lm.loadNative();
        try {
            lm.login();
            int appID = lm.getAppid();
            System.out.println("App ID = " + appID);
            int sessionID = lm.getSessionid();
            System.out.println("Session ID = " + sessionID);
        } catch (CFM2Exception e) {
            e.printStackTrace();
        }
    }
    
/**
 * Method #3: Use environment variables
 *
 * Before invoking the JVM, set the following environment variables
 * using a valid user name and password
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
            System.out.println("App ID = " + appID);
            int sessionID = lm.getSessionid();
            System.out.println("Session ID = " + sessionID);
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

## Generating a Symmetric Key<a name="java-sample-symmetric-key"></a>

This example shows how to generate a 256\-bit Advanced Encryption Standard \(AES\) symmetric key and save it in an HSM\. By default, the keys that the HSM generates are not saved in the HSM \("persistent"\)\. To make a key persistent, that is, to convert a *session key* into a *token key*, call the `Util.persistKey()` method\.

This example does not return any output, but you can save the key object and use the key handle in other operations\.

This example uses the `loginWithExplicitCredentials()` method of the `LoginLogoutExample` class to log in to the HSM\. You can substitute the login method that you prefer\. Also, the example assumes that the Cavium provider is included in your Java provider file\. If it is not, create an instance of the provider, and substitute it for the **Cavium** string\.


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\*  | 

```
package com.amazonaws.cloudhsm.examples;

import java.io.IOException;
import java.security.Key;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import java.security.SecureRandom;
import java.security.Security;

import javax.crypto.KeyGenerator;
import javax.crypto.SecretKey;

import com.cavium.cfm2.CFM2Exception;
import com.cavium.cfm2.Util;
import com.cavium.key.CaviumAESKey;
import com.cavium.key.CaviumKey;
import com.cavium.provider.CaviumProvider;

public class SymmetricKeyGeneration {
// Generate a 256-bit AES symmetric key and save it in the HSM
    public static void main(String[] args) {
        LoginLogoutExample.loginWithExplicitCredentials();
        new SymmetricKeyGeneration().generateAESKey(256, true);
        LoginLogoutExample.logout();
    }

    public Key generateAESKey(int keySize, boolean isPersistent) {
        KeyGenerator keyGen;
        try {
            keyGen = KeyGenerator.getInstance("AES","Cavium");
            keyGen.init(keySize);
            SecretKey aesKey = keyGen.generateKey();
            System.out.println("Generated the AES key");
            if(aesKey instanceof CaviumAESKey) {
                System.out.println("Key is of type CaviumAESKey");
                CaviumAESKey ck = (CaviumAESKey) aesKey;
                //Save the key handle. You'll need it to encrypt/decrypt in the future.
                System.out.println("Key handle = " + ck.getHandle());
                //Get the key label. The SDK generates this label for the key.
                System.out.println("Key label = " + ck.getLabel());
                System.out.println("Is the key extractable? : " + ck.isExtractable());
                
                //By default, keys are not saved in the HSM. 
                System.out.println("Is the key persistent? : " + ck.isPersistent());
                // Save the key in the HSM, if requested
                if(isPersistent){
                    System.out.println("Make the key persistent:");
                    makeKeyPersistent(ck);
                }
                System.out.println("Is key persistent? : " + ck.isPersistent());
                
                //Verify key type and size
                System.out.println("Key algorithm : " + ck.getAlgorithm());
                System.out.println("Key size : " + ck.getSize());
            }
            // Return the key
            return aesKey;
        } catch (NoSuchAlgorithmException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (NoSuchProviderException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } 
        return null;
    }
    
    public static void makeKeyPersistent(Key key) {
        CaviumAESKey caviumAESKey = (CaviumAESKey) key;
        try {
            Util.persistKey(caviumAESKey);
            System.out.println("Added Key to HSM");
        } catch (CFM2Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

## Encrypting and Decrypting with a Symmetric Key<a name="java-sample-symmetric-encrypt-decrypt"></a>

This example shows how to encrypt and decrypt a string using a 256\-bit Advanced Encryption Standard \(AES\) symmetric key\. 

The example uses the AES algorithm with Galois Counter Mode \(GCM\), which uses authenticated encryption with associated data \(AEAD\)\. The code specifies an additional authenticated data \(AAD\) string and an initialization vector \(IV\), which is an arbitrary number, like a nonce\. The encryption operation changes the AAD and IV, so you need to save the new AAD and IV and use them to decrypt the ciphertext\. The `encrypt()` method in this example returns an object \(`FinalResult`\) that includes the ciphertext, the IV \(and its length\) and the AAD \(and its length\)\.

To generate the symmetric key, this example calls the `generateAESKey()` method of the `SymmetricKeyGeneration` class in the [[ERROR] BAD/MISSING LINK TEXT](#java-sample-symmetric-key) example\. It uses the `loginWithExplicitCredentials()` method in the `LoginLogoutExample` class to log in to the HSM, but you can substitute the login method that you prefer\. Also, the example assumes that the Cavium provider is included in your Java provider file\. If it is not, create an instance of the provider and substitute it for the `Cavium` string\.


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\*  | 

```
package com.amazonaws.cloudhsm.examples.crypto.symmetric;

import java.security.InvalidAlgorithmParameterException;
import java.security.InvalidKeyException;
import java.security.Key;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import java.security.SecureRandom;
import java.util.Arrays;
import java.util.Base64;

import javax.crypto.BadPaddingException;
import javax.crypto.Cipher;
import javax.crypto.IllegalBlockSizeException;
import javax.crypto.NoSuchPaddingException;
import javax.crypto.spec.GCMParameterSpec;

import com.amazonaws.cloudhsm.examples.key.symmetric.SymmetricKeyGeneration;
import com.amazonaws.cloudhsm.examples.operations.LoginLogoutExample;
import com.cavium.key.CaviumAESKey;

public class SymmetricEncryptDecryptExample {

    String plainText = "This is a sample plaintext message";
    String aad = "AAD data";
    String transformation = "AES/GCM/NoPadding";
    int ivSizeInBytes=12;
    int tagLengthInBytes = 16;
    /*
     * AEAD modes, such as GCM and CCM, authenticate the AAD before authenticating the ciphertext. 
     * To avoid buffering the ciphertext internally, supply all AAD data to GCM/CCM implementations 
     * before the ciphertext is processed.
     */
    public static void main(String[] args) {

        SymmetricEncryptDecryptExample obj = new SymmetricEncryptDecryptExample();
        LoginLogoutExample.loginWithExplicitCredentials();
        // Generate an 256-bit AES key and save it in the HSM
        Key key =new SymmetricKeyGeneration().generateAESKey(256, true);
        // Generate an initialization vector (IV)
        byte[] iv = obj.generateIV(obj.ivSizeInBytes);

        System.out.println("Performing AES encryption operation");
        // Encrypt the plaintext with the specified algorithm, key, IV, and the AAD
        byte[] result = obj.encrypt(obj.transformation, (CaviumAESKey) key, obj.plainText, iv, obj.aad, obj.tagLengthInBytes);
        System.out.println("Plaintext is encrypted");
        System.out.println("Base64-encoded encrypted text = " + Base64.getEncoder().encodeToString(result));
        System.out.println("Decrypting the ciphertext");
        //Extract the IV for the decrypt operation
        iv = Arrays.copyOfRange(result, 0, 16);
        byte[] cipherText = Arrays.copyOfRange(result, 16, result.length);

        // Decrypt the ciphertext using the algorithm, key, IV, and AAD
        byte[] decryptedText = obj.decrypt(obj.transformation, (CaviumAESKey) key, cipherText, iv, obj.aad, obj.tagLengthInBytes);
        System.out.println("Plaintext = "+new String(decryptedText));
        LoginLogoutExample.logout();
    }
    // This encrypt operation uses an initialization vector (IV) and additional authenticated data (AAD)
    public byte[] encrypt(String transformation, CaviumAESKey key, String plainText, byte[] iv, String aad, int tagLength) {
        try {
            // Create an encryption cipher
            Cipher encCipher = Cipher.getInstance(transformation, "Cavium");
            // Create a parameter spec
            GCMParameterSpec gcmSpec = new GCMParameterSpec(tagLengthInBytes * 8, iv);
            // Configure the encryption cipher
            encCipher.init(Cipher.ENCRYPT_MODE, key, gcmSpec);
            encCipher.updateAAD(aad.getBytes());
            encCipher.update(plainText.getBytes());
            // Encrypt the plaintext data
            byte[] ciphertext = encCipher.doFinal();
            
            //Save the new IV and AADTag from the HSM. 
            //You'll need them to create the GCMParameterSpec for the decrypt operation.
            byte[] finalResult = new byte[encCipher.getIV().length + ciphertext.length];
            System.arraycopy(encCipher.getIV(), 0, finalResult, 0, encCipher.getIV().length);
            System.arraycopy(ciphertext, 0, finalResult, encCipher.getIV().length, ciphertext.length);
            return finalResult;

        } catch (NoSuchAlgorithmException | NoSuchProviderException | NoSuchPaddingException e) {
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (InvalidAlgorithmParameterException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IllegalBlockSizeException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (BadPaddingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
    // Generate an initialization vector (IV)
    public byte[] generateIV(int ivSizeinByets) {
        SecureRandom sr;
        try {
            sr = SecureRandom.getInstance("AES-CTR-DRBG", "Cavium");
            byte[] iv = new byte[ivSizeinByets];
            sr.nextBytes(iv);  
            return iv;
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
    //Decrypt with the specified algorithm, key, ciphertext, IV, and AAD.
    public byte[] decrypt(String transformation, CaviumAESKey key, byte[] cipherText , byte[] iv, String aad, int tagLength) {
        Cipher decCipher;
        try {
            //Create the decryption cipher
            decCipher = Cipher.getInstance(transformation, "Cavium");
            // Create a Cavium parameter spec from the IV and AAD
            GCMParameterSpec gcmSpec = new GCMParameterSpec(tagLengthInBytes * 8,iv);
            //Configure the decryption cipher
            decCipher.init(Cipher.DECRYPT_MODE, key, gcmSpec);
            decCipher.updateAAD(aad.getBytes());
            //Decrypt the ciphertext and return the plaintext
            return decCipher.doFinal(cipherText);

        } catch (NoSuchAlgorithmException | NoSuchProviderException | NoSuchPaddingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (InvalidAlgorithmParameterException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IllegalBlockSizeException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (BadPaddingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

## Generating an Asymmetric Key Pair<a name="java-sample-asymmetric-key"></a>

This example shows how to generate an RSA key pair and then save the public and private keys in the HSM\. To make a key persistent, that is, to convert a *session key* into a *token key*, call the `Util.persistKey()` method\.

The example does not return any output, but you can save the key pair object and use the public and private key handles in other operations\.

To log in to the HSM, this example uses the `loginWithExplicitCredentials()` method of the `LoginLogoutExample` class, but you can substitute the login method that you prefer\. Also, the example assumes that the Cavium provider is included in your Java provider file\. If it is not, create an instance of the provider, and substitute it for the **Cavium** string\.


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\*  | 

```
package com.amazonaws.cloudhsm.examples;

import java.math.BigInteger;
import java.security.InvalidAlgorithmParameterException;
import java.security.Key;
import java.security.KeyPair;
import java.security.KeyPairGenerator;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import java.security.interfaces.RSAPrivateKey;
import java.security.interfaces.RSAPublicKey;

import com.cavium.cfm2.CFM2Exception;
import com.cavium.cfm2.Util;
import com.cavium.key.CaviumAESKey;
import com.cavium.key.CaviumKey;
import com.cavium.key.CaviumRSAPrivateKey;
import com.cavium.key.CaviumRSAPublicKey;
import com.cavium.key.parameter.CaviumRSAKeyGenParameterSpec;

public class AsymmetricKeyGeneration {
    
// Generate a 2048-bit RSA key pair and save it in the HSM
    public static void main(String[] args) {
        LoginLogoutExample.loginWithExplicitCredentials();
        new AsymmetricKeyGeneration().generateRSAKeyPair(2048, true);
        LoginLogoutExample.logout();
    }
    
    public KeyPair generateRSAKeyPair(int keySize, boolean isPersistent) {
        KeyPairGenerator keyPairGen;
        try {
            // Create and configure a key pair generator
            keyPairGen = KeyPairGenerator.getInstance("rsa", "Cavium");
            keyPairGen.initialize(new CaviumRSAKeyGenParameterSpec(keySize, new BigInteger("65537")));
            // Generate the key pair
            KeyPair kp = keyPairGen.generateKeyPair();
            
            if (kp == null) {
                System.out.println("Failed to generate key pair");
            }
            RSAPrivateKey privKey = (RSAPrivateKey) kp.getPrivate();
            RSAPublicKey pubKey = (RSAPublicKey) kp.getPublic();
            System.out.println("Generated RSA key pair");
            //Write out properties of RSA private key
            if (privKey instanceof CaviumRSAPrivateKey) {
                CaviumRSAPrivateKey cavRSAPrivateKey = (CaviumRSAPrivateKey) privKey;
                System.out.println("Private key handle = " + cavRSAPrivateKey.getHandle());
                System.out.println("Private key label = " + cavRSAPrivateKey.getLabel());
                System.out.println("Is private key extractable = " + cavRSAPrivateKey.isExtractable());
                System.out.println("Is private key persistent = " + cavRSAPrivateKey.isPersistent());
                
                // Save RSA private key in HSM, if requested
                if(isPersistent) {
                    makeKeyPersistent(cavRSAPrivateKey);
                    System.out.println("Added RSA private key to HSM");
                }
                System.out.println("Modulus = " + cavRSAPrivateKey.getModulus());
                System.out.println("Private exponent = " + cavRSAPrivateKey.getPrivateExponent());
            }
            //Write out properties of RSA public key
            if(pubKey instanceof CaviumRSAPublicKey) {
                CaviumRSAPublicKey cavRSAPublicKey = (CaviumRSAPublicKey) pubKey;
                //Save the key handle. You'll need it to encrypt/decrypt in the future.
                System.out.println("Public key handle = " + cavRSAPublicKey.getHandle());
                System.out.println("Public key label = " + cavRSAPublicKey.getLabel());
                System.out.println("Is public key extractable = " +cavRSAPublicKey.isExtractable());
                System.out.println("Is public key persistent = " + cavRSAPublicKey.isPersistent());
                
                // Save RSA public key in HSM, if requested
                if(isPersistent) {
                    makeKeyPersistent(cavRSAPublicKey);
                    System.out.println("Added RSA public key to HSM");
                }
                System.out.println("Added RSA public key to HSM");
                System.out.println("Modulus = " + cavRSAPublicKey.getModulus());
                System.out.println("Public exponent = " + cavRSAPublicKey.getPublicExponent());
            }
            // Return the key pair
            return kp;
        } catch (NoSuchAlgorithmException | NoSuchProviderException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (InvalidAlgorithmParameterException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
    // Save key in HSM
    protected void makeKeyPersistent(CaviumKey key) {
        CaviumKey rsaKey = (CaviumKey) key;
        try {
            Util.persistKey(rsaKey);
        } catch (CFM2Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

## Encrypting and Decrypting with an Asymmetric Key Pair<a name="java-sample-asymmetric-encrypt-decrypt"></a>

This example shows how to encrypt and decrypt a string using a 2048\-bit RSA key pair\. First it encrypts with the public key and decrypts with the private key\. Then it encrypts with the private key and decrypts with the public key\. 

To generate the key pair, this example calls the `generateRSAKeyPair()` method of the `AsymmetricKeyGeneration` class in the [[ERROR] BAD/MISSING LINK TEXT](#java-sample-asymmetric-key) example\. It uses the `loginWithExplicitCredentials()` method in the `LoginLogoutExample` class to log in to the HSM, but you can substitute the login method that you prefer\. Also, the example assumes that the Cavium provider is included in your Java provider file\. If it is not, create an instance of the provider and substitute it for the **Cavium**s string\.


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\*  | 

```
package com.amazonaws.cloudhsm.examples;

import java.security.InvalidKeyException;
import java.security.Key;
import java.security.KeyPair;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import java.security.interfaces.RSAPrivateKey;
import java.security.interfaces.RSAPublicKey;

import javax.crypto.BadPaddingException;
import javax.crypto.Cipher;
import javax.crypto.IllegalBlockSizeException;
import javax.crypto.NoSuchPaddingException;

import org.bouncycastle.util.encoders.Base64;

import com.cavium.key.CaviumRSAPrivateKey;
import com.cavium.key.CaviumRSAPublicKey;

public class AsymmetricEncryptDecryptExample {
    
    String plainText = "This is a plaintext string";
    // Specify the encryption algorithm
    String transformation  = "RSA/ECB/OAEPWithSHA-224ANDMGF1Padding";
    public static void main(String[] args) {
        // Log into the HSM
        LoginLogoutExample.loginWithExplicitCredentials();
        // Generate a 2048-bit RSA key pair and save it in the HSM
        KeyPair kp = new AsymmetricKeyGeneration().generateRSAKeyPair(2048, true);
        // Create an example object
        AsymmetricEncryptDecryptExample obj = new AsymmetricEncryptDecryptExample();
        //Get the private key
        CaviumRSAPrivateKey privKey = (CaviumRSAPrivateKey) (RSAPrivateKey) kp.getPrivate();
        //Get the public key
        CaviumRSAPublicKey pubKey = (CaviumRSAPublicKey) (RSAPublicKey) kp.getPublic();
        System.out.println("Use the private key to encrypt; use the public key to decrypt");
        // Encrypt the plaintext with the private key
        byte[] cipherText = obj.asymmetricKeyEncryption(obj.transformation, privKey, obj.plainText);
        System.out.println("CipherText = " + Base64.toBase64String(cipherText));
        // Decrypt the ciphertext with the public key
        String plainText = obj.asymmetricKeyDecryption(obj.transformation, pubKey, cipherText);
        System.out.println("PlainText = " + plainText);
        System.out.println("Encrypt with public key; decrypt with private key");
        // Encrypt with the public key
        cipherText = obj.asymmetricKeyEncryption(obj.transformation, pubKey, obj.plainText);
        System.out.println("CipherText = " + Base64.toBase64String(cipherText));
        // Decrypt with private key
        plainText = obj.asymmetricKeyDecryption(obj.transformation, privKey, cipherText);
        System.out.println("PlainText = " + plainText);
        LoginLogoutExample.logout();
    }
// Encrypt with the specified algorithm and key
    public byte[] asymmetricKeyEncryption(String transformation, Key key, String plainText) { 
        try {
            Cipher cipher = Cipher.getInstance(transformation, "Cavium");
            cipher.init(Cipher.ENCRYPT_MODE, key);
            cipher.update(plainText.getBytes());
            // Encrypt the plaintext
            byte[] cihperText = cipher.doFinal(plainText.getBytes());
            return cihperText;
        } catch (NoSuchAlgorithmException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (NoSuchProviderException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (NoSuchPaddingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IllegalBlockSizeException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (BadPaddingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
// Decrypt with the specified algorithm and key
    public String asymmetricKeyDecryption(String transformation, Key key, byte[] cipherText) { 
        try {
            Cipher cipher = Cipher.getInstance(transformation, "Cavium");
            cipher.init(Cipher.DECRYPT_MODE, key);
            // Decrypt the ciphertext
            byte[] plainText = cipher.doFinal(cipherText);
            return new String(plainText);
        } catch (NoSuchAlgorithmException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (NoSuchProviderException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (NoSuchPaddingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IllegalBlockSizeException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (BadPaddingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

## Signing a Message<a name="java-sample-sign-message"></a>

This example shows how to sign a message with a key in an HSM\. The example generates a 4096\-bit asymmetric key pair\. It uses the private key to sign the message\. Then it uses the public key to verify the message signature\. 

To generate the key pair, this example calls the `generateRSAKeyPair()` method of the `AsymmetricKeyGeneration` class in the [[ERROR] BAD/MISSING LINK TEXT](#java-sample-asymmetric-key) example\. It uses the `loginWithExplicitCredentials()` method in the `LoginLogoutExample` class to log in to the HSM, but you can substitute the login method that you prefer\. Also, the example assumes that the Cavium provider is included in your Java provider file\. If it is not, create an instance of the provider, and substitute it for the **Cavium** string\.


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\*  | 

```
package com.amazonaws.cloudhsm.examples;

import java.security.InvalidKeyException;
import java.security.Key;
import java.security.KeyPair;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import java.security.Signature;
import java.security.SignatureException;
import java.util.Base64;

import com.cavium.key.CaviumRSAPrivateKey;
import com.cavium.key.CaviumRSAPublicKey;

public class SignatureExample {
    
    String sampleMessage = "This is a sample message.";
    String signingAlgorithmn = "SHA512withRSA/PSS";
    public static void main(String[] args) {
        LoginLogoutExample.loginUsingJavaProperties();
        SignatureExample obj = new SignatureExample();
        
        //Generate a 4096-bit pair and save it in the HSM 
        KeyPair kp = new AsymmetricKeyGeneration().generateRSAKeyPair(4096, true);
        System.out.println("Generated key pair");
        
        //Sign the message with the private key and the specified signing algorithm
        byte[] signature = obj.signMessage(obj.sampleMessage, obj.signingAlgorithm, (CaviumRSAPrivateKey)kp.getPrivate());
        System.out.println("Signature : " + Base64.getEncoder().encodeToString(signature));
        
        //Verify the signature
        boolean isVerificationSuccessful = obj.verifySign(obj.sampleMessage, obj.signingAlgo, (CaviumRSAPublicKey)kp.getPublic(), signature);
        System.out.println("Verification result : " + isVerificationSuccessful);
        LoginLogoutExample.logout();
    }
    //Use the private key to sign the message
    public byte[] signMessage(String message, String signingAlgorithm, CaviumRSAPrivateKey privateKey) {
        try {
            Signature sig = Signature.getInstance(signingAlgorithm, "Cavium");
            sig.initSign(privateKey);
            sig.update(message.getBytes()); 
            byte[] signature = null;
            signature = sig.sign();
            return signature;

        } catch (NoSuchAlgorithmException | NoSuchProviderException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (SignatureException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
    //Use the public key to verify the message signature
    public boolean verifySign(String message, String signingAlgorithm, CaviumRSAPublicKey publicKey, byte[] signature) {
        try {
            Signature sig = Signature.getInstance(signingAlgorithm, "Cavium");
            sig.initVerify(publicKey);
            sig.update(message.getBytes());
            
            boolean isVerificationSuccessful = sig.verify(signature);
            return isVerificationSuccessful;
        } catch (NoSuchAlgorithmException | NoSuchProviderException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (SignatureException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return false;
    }

}
```

## Generating a Hash<a name="java-sample-hash"></a>

This example shows how to generate a hash of a message using an HSM and the SHA\-512 hash algorithm\.

To log in to the HSM, this example uses the `loginWithExplicitCredentials()` method of the `LoginLogoutExample` class, but you can substitute the login method that you prefer\. Also, the example assumes that the Cavium provider is included in your Java provider file\. If it is not, create an instance of the provider and substitute it for the **Cavium** string\.


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\*  | 

```
package com.amazonaws.cloudhsm.examples;

import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import javax.xml.bind.DatatypeConverter;

public class HashExample {
    String plainText = "This is a sample plaintext message.";
    String hashAlgorithm = "SHA-512";
    public static void main(String[] args) {
        LoginLogoutExample.loginWithExplicitCredentials();
        HashExample obj = new HashExample();
        // Generate the hash
        byte[] hash = obj.getHash(obj.plainText, obj.hashAlgorithm);

        System.out.println("Hash : " + DatatypeConverter.printHexBinary(hash));
        LoginLogoutExample.logout();
    }

    public byte[] getHash(String message, String hashAlgorithm) {
        try {
            // Specify the Cavium provider
            MessageDigest md = MessageDigest.getInstance(hashAlgorithm, "Cavium");
            md.update(message.getBytes());
            byte[] hash = md.digest();
            return hash;
        } catch (NoSuchAlgorithmException | NoSuchProviderException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

## Generating an HMAC<a name="java-sample-hmac"></a>

This example shows how to generate a hash\-based message authentication code \(HMAC\) in the HSM and use it to hash a message\. Unlike a typical hash, an HMAC uses a hash function and a cryptographic key\.

To generate the key pair, this example calls the `generateRSAKeyPair()` method of the `AsymmetricKeyGeneration` class in the [[ERROR] BAD/MISSING LINK TEXT](#java-sample-asymmetric-key) example\. It uses the `loginWithExplicitCredentials()` method in the `LoginLogoutExample` class to log in to the HSM, but you can substitute the login method that you prefer\. Also, the example assumes that the Cavium provider is included in your Java provider file\. If it is not, create an instance of the provider and substitute it for the **Cavium** string\.


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\*  | 

```
package com.amazonaws.cloudhsm.examples;

import java.security.InvalidKeyException;
import java.security.Key;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;

import javax.crypto.Mac;
import javax.xml.bind.DatatypeConverter;

import com.cavium.key.CaviumAESKey;

public class HMACExample {

    String message  = "This is a plaintext message.";
    String macAlgorithm= "HmacSHA512";

    public static void main(String[] args) {
        LoginLogoutExample.loginWithExplicitCredentials();
        // Generate a 256-bit AES key for the HMAC
        Key aesKey = new SymmetricKeyGeneration().generateAESKey(256, true);
        HMACExample obj = new HMACExample();
        
        // Generate the HMAC using the plaintext, algorithm, and key
        byte[] mac= obj.getHmac(obj.message, obj.macAlgorithm, (CaviumAESKey)aesKey);
        System.out.println("HMAC : " + DatatypeConverter.printHexBinary(mac));
        LoginLogoutExample.logout();
    }
    // The HMAC function takes a hash algorithm and a cryptographic key
    public byte[] getHmac(String message, String macAlgorithm, CaviumAESKey key) {
        try {
            Mac mac = Mac.getInstance( macAlgorithm,"Cavium");
            mac.init(key);
            mac.update(message.getBytes());
            byte[] hmacValue = mac.doFinal();
            return hmacValue;

        } catch (NoSuchAlgorithmException | NoSuchProviderException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;

    }
}
```

## Managing Keys in an HSM<a name="java-sample-manage-keys"></a>

This example shows how to manage keys in an HSM\. It demonstrates the following operations:

+ **Get** a reference to a key in the HSM\.

+ **Export** a key from the HSM\. This operation returns the key, not just a reference, so you can import the key into a different HSM and use it in other operations\. It does not delete the key from the HSM\.

+ **Delete** a key from the HSM\.

+ **Import** a key into the HSM\. This example returns a key handle that you can use to identify the key in other operations\.

**Note**  
To use a key in an encryption operation, just specify the key handle\. You do not need to get or export the key\.

To log into the HSM, this example uses the `loginWithExplicitCredentials()` method of the `LoginLogoutExample` class\.

To generate the key pair, this example calls the `generateRSAKeyPair()` method of the `AsymmetricKeyGeneration` class in the [[ERROR] BAD/MISSING LINK TEXT](#java-sample-asymmetric-key) example\. It uses the `loginWithExplicitCredentials()` method in the `LoginLogoutExample` class to log in to the HSM, but you can substitute the login method that you prefer\. Also, the example assumes that the Cavium provider is included in your Java provider file\. If it is not, create an instance of the provider and substitute it for the **Cavium** string\.

You can also use the **key\_mgmt\_util** command line tool to manage keys in AWS CloudHSM\.


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\*  | 

```
package com.amazonaws.cloudhsm.examples;

import java.security.InvalidKeyException;
import java.security.Key;
import java.security.KeyFactory;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import java.security.PrivateKey;
import java.security.PublicKey;
import java.security.spec.InvalidKeySpecException;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.spec.X509EncodedKeySpec;
import java.util.Base64;
import java.util.Vector;

import javax.crypto.BadPaddingException;
import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import javax.crypto.KeyGenerator;

import org.bouncycastle.util.Arrays;

import com.cavium.cfm2.CFM2Exception;
import com.cavium.cfm2.ImportKey;
import com.cavium.cfm2.Util;
import com.cavium.key.CaviumAESKey;
import com.cavium.key.CaviumKey;
import com.cavium.key.CaviumKeyAttributes;
import com.cavium.key.CaviumRSAPrivateKey;
import com.cavium.key.CaviumRSAPublicKey;
import com.cavium.key.parameter.CaviumKeyGenAlgorithmParameterSpec;

public class KeyManagement {

    public static void main(String[] args) {
        LoginLogoutExample.loginWithExplicitCredentials();
        //Get a reference to a key in the HSM
        //  Replace the placeholder with an actual key handle value
        long keyHandle =262194;    
        CaviumKey ck = getKey(keyHandle);
        
        //Delete the specified key from the HSM
        //  Replace the placeholder with an actual key handle value
        deleteKey(51);
        Key key = exportKey(keyHandle);
        
        //Import a key
        //  Generate a 256-bit AES symmetric key
        KeyGenerator kg = KeyGenerator.getInstance("AES");
        kg.init(256);
        Key keyToBeImported = kg.generateKey();
        //Import the key as extractable and persistent
        //You can use the key handle to identify the key in other operations
        long importedKeyHandle = importKey(keyToBeImported, "Test", true, true);
        System.out.println("Imported Key Handle : " + importedKeyHandle);

        LoginLogoutExample.logout();
    }
    //Gets an existing key from the HSM
    //The type of the object that is returned dependes on the key type
    public static CaviumKey getKey(long handle) {
        try {
            byte[] keyAttribute = Util.getKeyAttributes(handle);
            CaviumKeyAttributes cka = new CaviumKeyAttributes(keyAttribute);
            if(cka.getKeyType() == CaviumKeyAttributes.KEY_TYPE_AES) {
                CaviumAESKey aesKey = new CaviumAESKey(handle, cka);
                return aesKey;
            }
            else if(cka.getKeyType() == CaviumKeyAttributes.KEY_TYPE_RSA && cka.getKeyClass() == CaviumKeyAttributes.CLASS_PRIVATE_KEY) {
                CaviumRSAPrivateKey privKey = new CaviumRSAPrivateKey(handle, cka);
                return privKey;
            }
            else if(cka.getKeyType() == CaviumKeyAttributes.KEY_TYPE_RSA && cka.getKeyClass() == CaviumKeyAttributes.CLASS_PUBLIC_KEY) {
                CaviumRSAPublicKey pubKey = new CaviumRSAPublicKey(handle, cka);
                return pubKey;
            }
        } catch (CFM2Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;        
    }
    //Deletes an existing persisted key
    public static void deteleKey(long handle) {
        CaviumKey ck = getKey(handle);
        try {
            Util.deleteKey(ck);
            System.out.println("Key Deleted!");
        } catch (CFM2Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
    //Exports an existing persisted key
    //The type of the object that is returned depends on the key type
    public static Key exportKey(long handle) {
        try {
            byte[] encoded = Util.exportKey( handle);
            byte[] keyAttribute = Util.getKeyAttributes(handle);
            CaviumKeyAttributes cka = new CaviumKeyAttributes(keyAttribute);
            if(cka.getKeyType() == CaviumKeyAttributes.KEY_TYPE_AES) {
                Key aesKey = new SecretKeySpec(encoded, 0, encoded.length, "AES");
                return aesKey;
            }
            else if(cka.getKeyType() == CaviumKeyAttributes.KEY_TYPE_RSA && cka.getKeyClass() == CaviumKeyAttributes.CLASS_PRIVATE_KEY) {
                PrivateKey privateKey = KeyFactory.getInstance("RSA").generatePrivate(new PKCS8EncodedKeySpec(encoded));
                return privateKey;
            }
            else if(cka.getKeyType() == CaviumKeyAttributes.KEY_TYPE_RSA && cka.getKeyClass() == CaviumKeyAttributes.CLASS_PUBLIC_KEY) {
                PublicKey publicKey = KeyFactory.getInstance("RSA").generatePublic(new X509EncodedKeySpec(encoded));
                return publicKey;
            }
            System.out.println(new String(encoded));
        } catch (BadPaddingException | CFM2Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (InvalidKeySpecException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } 
        return null;
    }
    //Imports a key explicitly
    public static void importKey(Key key, String keyLabel, boolean isExtractable, boolean isPersistent) {
        //Create a new key parameter spec to identify the key. Specify a label and Boolean values for extractable and persistent.
        CaviumKeyGenAlgorithmParameterSpec spec = new CaviumKeyGenAlgorithmParameterSpec(keyLabel, isExtractable, isPersistent);
        try {
            ImportKey.importKey(key, spec);
        } catch (InvalidKeyException e) {
            e.printStackTrace();
        }
    }
    
}
```