# Generate SMTP Credentials From Existing IAM Credentials<a name="example-create-smtp-credentials"></a>

To send email by using the Amazon SES SMTP interface, you have to create an SMTP username and password\. The easiest way to create an SMTP user name and password is to use the Amazon SES console\. For more information, see [Obtaining Amazon SES SMTP Credentials Using the Amazon SES Console](smtp-credentials.md#smtp-credentials-console)\.

Some programming languages include libraries that you can use to convert an IAM secret access key into an SMTP password\. If you already have an IAM user that you want to use to send email through the SMTP interface, you can use these code examples to convert the AWS secret access key for that user into an SMTP password\.

Before you execute these examples, put the AWS Secret Access Key that you want to convert into an environment variable called `AWS_SECRET_ACCESS_KEY`\. These code examples pass your converted SMTP password as their output\. This password, and the SMTP username \(which is the same as your AWS access key ID\) are your Amazon SES SMTP credentials\. 

------
#### [ Bash ]

```
 1. #!/usr/bin/env bash
 2. 
 3. # These variables are required to calculate the SMTP password.
 4. VERSION='\x02'
 5. MESSAGE='SendRawEmail'
 6. 
 7. # Check to see if OpenSSL is installed. If not, exit with errors.
 8. if ! [[ -x "$(command -v openssl)" ]]; then
 9.   echo "Error: OpenSSL isn't installed." >&2
10.   exit 1
11. # If OpenSSL is installed, check to see that the environment variable has a
12. # length greater than 0. If not, exit with errors.
13. elif [[ -z "${AWS_SECRET_ACCESS_KEY}" ]]; then
14.   echo "Error: Couldn't find environment variable AWS_SECRET_ACCESS_KEY." >&2
15.   exit 1
16. fi
17. 
18. # If we made it this far, all of the required elements exist.
19. # Calculate the SMTP password.
20. (echo -en $VERSION; echo -n $MESSAGE \
21.  | openssl dgst -sha256 -hmac $AWS_SECRET_ACCESS_KEY -binary) \
22.  | openssl enc -base64
```

------
#### [ Java ]

```
 1. import javax.crypto.Mac;
 2. import javax.crypto.spec.SecretKeySpec;
 3. import javax.xml.bind.DatatypeConverter;
 4. 
 5. public class SesSmtpCredentialGenerator {
 6.    // Put your AWS secret access key in this environment variable.
 7.    private static final String KEY_ENV_VARIABLE = "AWS_SECRET_ACCESS_KEY"; 
 8.    // Used to generate the HMAC signature. Do not modify.
 9.    private static final String MESSAGE = "SendRawEmail"; 
10.    // Version number. Do not modify.
11.    private static final byte VERSION =  0x02; 
12. 
13.    public static void main(String[] args) {
14.         
15.       // Get the AWS secret access key from environment variable AWS_SECRET_ACCESS_KEY.
16.       String key = System.getenv(KEY_ENV_VARIABLE);             
17.       if (key == null)
18.       {
19.        System.out.println("Error: Cannot find environment variable AWS_SECRET_ACCESS_KEY.");  
20.        System.exit(0);
21.       }
22.            
23.       // Create an HMAC-SHA256 key from the raw bytes of the AWS secret access key.
24.       SecretKeySpec secretKey = new SecretKeySpec(key.getBytes(), "HmacSHA256");
25. 
26.       try {             
27.          // Get an HMAC-SHA256 Mac instance and initialize it with the AWS secret access key.
28.          Mac mac = Mac.getInstance("HmacSHA256");
29.          mac.init(secretKey);
30. 
31.          // Compute the HMAC signature on the input data bytes.
32.          byte[] rawSignature = mac.doFinal(MESSAGE.getBytes());
33. 
34.          // Prepend the version number to the signature.
35.          byte[] rawSignatureWithVersion = new byte[rawSignature.length + 1];               
36.          byte[] versionArray = {VERSION};                
37.          System.arraycopy(versionArray, 0, rawSignatureWithVersion, 0, 1);
38.          System.arraycopy(rawSignature, 0, rawSignatureWithVersion, 1, rawSignature.length);
39. 
40.          // To get the final SMTP password, convert the HMAC signature to base 64.
41.          String smtpPassword = DatatypeConverter.printBase64Binary(rawSignatureWithVersion);       
42.          System.out.println(smtpPassword);
43.       } 
44.       catch (Exception ex) {
45.          System.out.println("Error generating SMTP password: " + ex.getMessage());
46.       }             
47.    }
48. }
```

------
#### [ Python ]

```
 1. import os      #required to fetch environment varibles
 2. import hmac    #required to compute the HMAC key
 3. import hashlib #required to create a SHA256 hash
 4. import base64  #required to encode the computed key
 5. import sys     #required for system functions (exiting, in this case)
 6. 
 7. # Fetch the environment variable called AWS_SECRET_ACCESS_KEY, which contains
 8. # the secret access key for your IAM user.
 9. key = os.getenv('AWS_SECRET_ACCESS_KEY',0)
10. 
11. # These varibles are used when calculating the SMTP password. You shouldn't
12. # change them.
13. message = 'SendRawEmail'
14. version = '\x02'
15. 
16. # See if the environment variable exists. If not, quit and show an error.
17. if key == 0:
18.     sys.exit("Error: Can't find environment variable AWS_SECRET_ACCESS_KEY.")
19. 
20. # Compute an HMAC-SHA256 key from the AWS secret access key.
21. signatureInBytes = hmac.new(key.encode('utf-8'),message.encode('utf-8'),hashlib.sha256).digest()
22. # Prepend the version number to the signature.
23. signatureAndVersion = version.encode('utf-8') + signatureInBytes
24. # Base64-encode the string that contains the version number and signature.
25. smtpPassword = base64.b64encode(signatureAndVersion)
26. # Decode the string and print it to the console.
27. print(smtpPassword.decode('utf-8'))
```

------