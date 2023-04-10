# Rotate keys for users with MFA enabled<a name="rotate-mfa-cloudhsm-cli"></a>

Follow these steps to rotate keys for users with MFA enabled\.

1. Use CloudHSM CLI to log in to the HSM as any admin or as the specific user who has MFA enabled \(see [Log in users with MFA enabled]() for details\)\.

1. Next, execute the command to change you MFA strategy\. You must provide the parameter `--token`\. This parameter specifies a file that will have unsigned tokens written to it\.

   ```
   aws-cloudhsm > user change-mfa token-sign --token unsigned-tokens.json --username <USERNAME> --role crypto-user --change-quorum
   Enter password:
   Confirm password:
   ```

1. Identify the file with unsigned tokens that need to be signed: `unsigned-tokens.json`\. The number of tokens in this file depends on the number of HSMs in your cluster\. Each token represents one HSM\. This file is JSON formatted and contains tokens that need to be signed to prove you have a private key\. This will be the new private key from the new RSA public/private key pair you wish to use for rotating the currently registered public key\.

   ```
   $cat unsigned-tokens.json
   {
     "version": "2.0",
     "tokens": [
       {
         "unsigned": "Vtf/9QOFY45v/E1osvpEMr59JsnP/hLDm4ItOO2vqL8=",
         "signed": ""
       },
       {
         "unsigned": "wVbC0/5IKwjyZK2NBpdFLyI7BiayZ24YcdUdlcxLwZ4=",
         "signed": ""
       },
       {
         "unsigned": "z6aW9RzErJBL5KqFG5h8lhTVt9oLbxppjod0Ebysydw=",
         "signed": ""
       }
     ]
   }
   ```

1. Sign these tokens with the private key you previously created during setup\. First we have to extract and decode the base64 encoded tokens\.

   ```
   $ echo "Vtf/9QOFY45v/E1osvpEMr59JsnP/hLDm4ItOO2vqL8=" > token1.b64
   $ echo "wVbC0/5IKwjyZK2NBpdFLyI7BiayZ24YcdUdlcxLwZ4=" > token2.b64
   $ echo "z6aW9RzErJBL5KqFG5h8lhTVt9oLbxppjod0Ebysydw=" > token3.b64
   $ base64 -d token1.b64 > token1.bin
   $ base64 -d token2.b64 > token2.bin
   $ base64 -d token3.b64 > token3.bin
   ```

1. You now have binary tokens\. Sign them using the RSA private key you previously created during setup\.

   ```
   $ openssl pkeyutl -sign \
         -inkey officer1.key \
         -pkeyopt digest:sha256 \
         -keyform PEM \
         -in token1.bin \
         -out token1.sig.bin
   $ openssl pkeyutl -sign \
         -inkey officer1.key \
         -pkeyopt digest:sha256 \
         -keyform PEM \
         -in token2.bin \
         -out token2.sig.bin
   $ openssl pkeyutl -sign \
         -inkey officer1.key \
         -pkeyopt digest:sha256 \
         -keyform PEM \
         -in token3.bin \
         -out token3.sig.bin
   ```

1. You now have binary signatures of the tokens\. Encode them using base64, and place them back in your token file\.

   ```
   $ base64 -w0 token1.sig.bin > token1.sig.b64
   $ base64 -w0 token2.sig.bin > token2.sig.b64 
   $ base64 -w0 token3.sig.bin > token3.sig.b64
   ```

1. Finally, copy and paste the base64 values back into your token file:

   ```
   {
     "version": "2.0",
     "tokens": [
       {
         "unsigned": "1jqwxb9bJOUUQLiNb7mxXS1uBJsEXh0B9nj05BqnPsE=",
         "signed": "eiw3fZeCKIY50C4zPeg9Rt90M1Qlq3WlJh6Yw7xXm4nF6e9ETLE39+9M+rUqDWMRZjaBfaMbg5d9yDkz5p13U7ch2tlF9LoYabsWutkT014KRq/rcYMvFsU9n/Ey/TK0PVaxLN42X+pebV4juwMhN4mK4CzdFAJgM+UGBOj4yB9recpOBB9K8QFSpJZALSEdDgUc/mS1eDq3rU0int6+4NKuLQjpR+LSEIWRZ6g6+MND2vXGskxHjadCQ09L7Tz8VcWjKDbxJcBiGKvkqyozl9zrGo8fA3WHBmwiAgS61Merx77ZGY4PFR37+j/YMSC14prCN15DtMRv2xA1SGSb4w=="
       },
       {
         "unsigned": "LMMFc34ASPnvNPFzBbMbr9FProS/Zu2P8zF/xzk5hVQ=",
         "signed": "HBImKnHmw+6R2TpFEpfiAg4+hu2pFNwn43ClhKPkn2higbEhUD0JVi+4MerSyvU/NN79iWVxDvJ9Ito+jpiRQjTfTGEoIteyuAr1v/Bzh+HjmrO53OQpZaJ/VXGIgApD0myuu/ZGNKQTCSkkL7+V81FG7yR1Nm22jUeGa735zvm/E+cenvZdy0VVx6A7WeWrl3JEKKBweHbi+7BwbaW+PTdCuIRd4Ug76Sy+cFhsvcG1k7cMwDh8MgXzIZ2m1f/hdy2j8qAxORTLlmwyUOYvPYOvUhc+s83hx36QpGwGcD7RA0bPT5OrTx7PHd0N1CL+Wwy91We8yIOFBS6nxo1R7w=="
       },
       {
         "unsigned": "dzeHbwhiVXQqcUGj563z51/7sLUdxjL93SbOUyZRjH8=",
         "signed": "VgQPvrTsvGljVBFxHnswduq16x8ZrnxfcYVYGf/N7gEzI4At3GDs2EVZWTRdvS0uGHdkFYp1apHgJZ7PDVmGcTkIXVD2lFYppcgNlSzkYlftr5EOjqS9ZjYEqgGuB4g//MxaBaRbJai/6BlcE92NIdBusTtreIm3yTpjIXNAVoeRSnkfuw7wZcL96QoklNb1WUuSHw+psUyeIVtIwFMHEfFoRC0t+VhmnlnFnkjGPb9W3Aprw2dRRvFM3R2ZTDvMCiOYDzUCd43GftGq2LfxH3qSD51oFHglHQVOY0jyVzzlAvub5HQdtOQdErIeO0/9dGx5yot07o3xaGl5yQRhwA=="
       }
     ]
   }
   ```

1. Now that your token file has all the required signatures, you can proceed\. Enter the name of the file containing the signed tokens and press the enter key\. Finally, enter the path of your new public key\. Now you will see the following as part of the output of [user list]()\.

   ```
   Enter signed token file path (press enter if same as the unsigned token file):
   Enter public key PEM file path:officer1.pub
   {
     "error_code": 0,
     "data": {
       "username": "<USERNAME>",
       "role": "crypto-user"
     }
   }
   ```

   Now we have setup our user with MFA\.

   ```
   {
       "username": "<USERNAME>",
       "role": "crypto-user",
       "locked": "false",
       "mfa": [
         {
           "strategy": "token-sign",
           "status": "enabled"
         }
       ],
       "cluster-coverage": "full"
   },
   ```

You have signed the generated JSON formatted token file with your private key and registered a new MFA public key\.