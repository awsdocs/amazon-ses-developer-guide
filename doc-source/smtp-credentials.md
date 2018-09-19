# Obtaining Your Amazon SES SMTP Credentials<a name="smtp-credentials"></a>

You need an Amazon SES SMTP user name and password to access the Amazon SES SMTP interface\. You can use the same set of SMTP credentials in all AWS regions\.

**Important**  
Your SMTP user name and password are different from your AWS access key ID and secret access key\. For more information about credentials, see [Using Credentials With Amazon SES](using-credentials.md)\.

There are two ways to generate your SMTP credentials\. You can either use the Amazon SES console or you can generate your SMTP credentials from your AWS credentials\.

Use the Amazon SES console to generate your SMTP credentials if:
+ You want to get your SMTP credentials using the easiest method\.
+ You don't need to automate SMTP credential generation using code or a script\.

Generate your SMTP credentials from your AWS credentials if:
+ You have an existing [AWS Identity and Access Management \(IAM\)](https://aws.amazon.com/iam/) user that you created using the IAM interface and you want that user to be able to send emails using the Amazon SES SMTP interface\.
+ You want to automate SMTP credential generation using code or a script\.

For information on each method, see [Obtaining Amazon SES SMTP Credentials Using the Amazon SES Console](#smtp-credentials-console) and [Obtaining Amazon SES SMTP Credentials by Converting AWS Credentials](#smtp-credentials-convert)\.

## Obtaining Amazon SES SMTP Credentials Using the Amazon SES Console<a name="smtp-credentials-console"></a>

When you generate SMTP credentials by using the Amazon SES console, the Amazon SES console creates an IAM user with the appropriate policies to call Amazon SES and provides you with the SMTP credentials associated with that user\. 

**Note**  
An IAM user can create Amazon SES SMTP credentials, but the IAM user's policy must give them permission to use IAM itself, because Amazon SES SMTP credentials are created by using IAM\. Your IAM policy must allow you to perform the following IAM actions: `iam:ListUsers`, `iam:CreateUser`, `iam:CreateAccessKey`, and `iam:PutUserPolicy`\. If you try to create Amazon SES SMTP credentials using the console and your IAM user doesn't have these permissions, you see an error that states that your account is "not authorized to perform iam:ListUsers\."

**To create your SMTP credentials**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, choose **SMTP Settings**\.

1. In the content pane, choose **Create My SMTP Credentials**\.

1. For **Create User for SMTP**, type a name for your SMTP user\. Alternatively, you can use the default value that is provided in this field\. When you finish, choose **Create**\.  
![\[Create User for SMTP\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/console_smtp_create_new_user.png)

1. Choose **Show User SMTP Credentials**\. Your SMTP credentials are shown on the screen\. Copy these credentials and store them in a safe place\. You can also choose **Download Credentials** to download a file that contains your credentials\.  
![\[Create User for SMTP\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/console_smtp_user_created.png)
**Important**  
This is the only time that you can view your SMTP credentials\. We recommend that you download these credentials and keep them in a location where they won't be deleted\. If you lose these credentials, you have restart the process of creating your SMTP user\.

1. Choose **Close Window**\.

If you want to delete your SMTP credentials, go to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/) and delete the IAM user name that corresponds with your SMTP credentials\. To learn more, go to the [Using IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_DeletingUserFromAccount.html) guide\.

If you want to change your SMTP password, go to the IAM console and delete your existing IAM user, and then go to the Amazon SES console to re\-generate your SMTP credentials\.

## Obtaining Amazon SES SMTP Credentials by Converting AWS Credentials<a name="smtp-credentials-convert"></a>

If you have an IAM user that you set up using the IAM interface, you can derive the user's Amazon SES SMTP credentials from their AWS credentials\.

**Important**  
Don't use temporary AWS credentials to derive SMTP credentials\. The Amazon SES SMTP interface doesn't support SMTP credentials that have been generated from temporary security credentials\. 

To enable the IAM user to send email using the Amazon SES SMTP interface, you need to do the following two steps:
+ Derive the user's SMTP credentials from their AWS credentials using the algorithm provided in this section\. Because you are starting from AWS credentials, the SMTP username is the same as the AWS access key ID, so you just need to generate the SMTP password\.
**Important**  
If you generate SMTP credentials using the Amazon SES console, the SMTP username isn't the same as the AWS access key ID\. The SMTP username and the AWS access key ID are only the same if you generate the SMTP password programmatically, as described in this section\.
+ Apply the following policy to the IAM user:

  ```
  1. { "Statement": [{
  2. 	"Effect":"Allow",
  3. 	"Action":"ses:SendRawEmail",
  4. 	"Resource":"*"
  5. }]}
  ```

  For more information about using Amazon SES with IAM, see [Controlling Access to Amazon SES](control-user-access.md)\.

**Note**  
Although you can generate Amazon SES SMTP credentials for any IAM user, we recommend that you create a separate IAM user when you generate your SMTP credentials\. For information about why it is good practice to create users for specific purposes, go to [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAMBestPractices.html)\.

The following pseudocode shows the algorithm that converts an AWS Secret Access Key to an Amazon SES SMTP password\.

```
1. key = AWS Secret Access Key;
2. message = "SendRawEmail";
3. versionInBytes = 0x02;
4. signatureInBytes = HmacSha256(message, key);
5. signatureAndVer = Concatenate(versionInBytes, signatureInBytes);
6. smtpPassword = Base64(signatureAndVer);
```

You can use OpenSSL to generate an SMTP password from an existing IAM Secret Access Key\. OpenSSL is an open\-source utility, and is compatible with all operating systems\. It is included by default in most versions of Linux, macOS, and Unix, and is also available for Windows\. To learn more about OpenSSL, see [https://www\.openssl\.org](https://www.openssl.org)\. To generate your SMTP password by using OpenSSL, type the following at the command line\. Replace *$AWS\_SECRET\_ACCESS\_KEY* with the secret access key for your IAM user\.

```
1. (echo -en "\x02"; echo -n 'SendRawEmail' \
2.   | openssl dgst -sha256 -hmac $AWS_SECRET_ACCESS_KEY -binary) \
3.   | openssl enc -base64
```

Some programming languages include libraries that you can use to convert an IAM secret access key into an SMTP password\. This section includes code examples that convert an AWS Secret Access Key to an Amazon SES SMTP password using Java or Python\. It also includes a Bash script that you can use to generate your SMTP password if OpenSSL is installed on your computer\.

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