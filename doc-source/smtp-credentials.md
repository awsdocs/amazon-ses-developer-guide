# Obtaining Your Amazon SES SMTP Credentials<a name="smtp-credentials"></a>

You need an Amazon SES SMTP user name and password to access the Amazon SES SMTP interface\. You can use the same set of SMTP credentials in all AWS regions\.

**Important**  
Your SMTP user name and password are not the same as your AWS access key ID and secret access key\. Do not attempt to use your AWS credentials to authenticate yourself against the SMTP endpoint\. For more information about credentials, see [Using Credentials With Amazon SES](using-credentials.md)\.

There are two ways to generate your SMTP credentials\. You can either use the Amazon SES console or you can generate your SMTP credentials from your AWS credentials\.

Use the Amazon SES console to generate your SMTP credentials if:
+ You want to get your SMTP credentials using the simplest method\.
+ You do not need to automate SMTP credential generation using code or a script\.

Generate your SMTP credentials from your AWS credentials if:
+ You have an existing [AWS Identity and Access Management \(IAM\)](https://aws.amazon.com/iam/) user that you created using the IAM interface and you want that user to be able to send emails using the Amazon SES SMTP interface\.
+ You want to automate SMTP credential generation using code or a script\.

For information on each method, see [Obtaining Amazon SES SMTP Credentials Using the Amazon SES Console](#smtp-credentials-console) and [Obtaining Amazon SES SMTP Credentials by Converting AWS Credentials](#smtp-credentials-convert)\.

## Obtaining Amazon SES SMTP Credentials Using the Amazon SES Console<a name="smtp-credentials-console"></a>

When you generate SMTP credentials by using the Amazon SES console, the Amazon SES console creates an IAM user with the appropriate policies to call Amazon SES and provides you with the SMTP credentials associated with that user\. 

**Note**  
An IAM user can create Amazon SES SMTP credentials, but the IAM user's policy must give them permission to use IAM itself, because Amazon SES SMTP credentials are created through IAM\. If the IAM user tries to create Amazon SES SMTP credentials using the console and they don't have IAM permissions, they will get an error that says "… not authorized to perform iam:ListUsers…" In that case, the root account owner needs to modify the IAM user's policy to allow them to access the following IAM actions: "iam:ListUsers", "iam:CreateUser", "iam:CreateAccessKey", and "iam:PutUserPolicy"\.

**To create your SMTP credentials**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, choose **SMTP Settings**\.

1. In the content pane, choose **Create My SMTP Credentials**\.

1. In the **Create User for SMTP** dialog box, you will see that an SMTP user name has been filled in for you\. You can accept this suggested user name or enter a different one\. To proceed, choose **Create**\.  
![\[Create User for SMTP\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/console_smtp_create_new_user.png)

1. Choose **Show User SMTP Credentials**\. Your SMTP credentials will be displayed on the screen; copy them and store them in a safe place\. You can also choose **Download Credentials** to download a file that contains your credentials\.  
![\[Create User for SMTP\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/console_smtp_user_created.png)
**Important**  
This is the only time that you will be able to view your SMTP credentials\! We strongly advise you to download these credentials and refrain from sharing them with others\.

1. Choose **Close Window**\.

If you want to delete your SMTP credentials, go to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/) and delete the IAM user name that corresponds with your SMTP credentials\. To learn more, go to the [Using IAM](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_DeletingUserFromAccount.html) guide\.

If you want to change your SMTP password, go to the IAM console and delete your existing IAM user, and then go to the Amazon SES console to re\-generate your SMTP credentials\.

## Obtaining Amazon SES SMTP Credentials by Converting AWS Credentials<a name="smtp-credentials-convert"></a>

If you have an IAM user that you set up using the IAM interface, you can derive the user's Amazon SES SMTP credentials from their AWS credentials\.

**Important**  
Do not use temporary AWS credentials to derive SMTP credentials\. The Amazon SES SMTP interface does not support SMTP credentials that have been generated from temporary security credentials\. 

To enable the IAM user to send email using the Amazon SES SMTP interface, you need to do the following two steps:
+ Derive the user's SMTP credentials from their AWS credentials using the algorithm provided in this section\. Because you are starting from AWS credentials, the SMTP username will be the same as the AWS access key ID, so you just need to generate the SMTP password\.
**Important**  
If you generate SMTP credentials using the Amazon SES console, the SMTP username is not the same as the AWS access key ID\. The SMTP username and the AWS access key ID are only the same if you generate the SMTP password programmatically, as described in this section\.
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
Although you can generate Amazon SES SMTP credentials for any existing IAM user, we recommend for security reasons that you create a separate IAM user for the AWS credentials that you will use to generate the SMTP password\. For information about why it is good practice to create users for specific purposes, go to [IAM Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/IAMBestPractices.html)\.

The following pseudocode shows the algorithm that converts an AWS Secret Access Key to an Amazon SES SMTP password\.

```
1. key = AWS Secret Access Key;
2. message = "SendRawEmail";
3. versionInBytes = 0x02;
4. signatureInBytes = HmacSha256(message, key);
5. signatureAndVer = Concatenate(versionInBytes, signatureInBytes);
6. smtpPassword = Base64(signatureAndVer);
```

If you have opensssl installed, the following bash oneliner uses that algorithm (after you replace `$secret_access_key` with your AWS Secret Access Key):

```bash
(echo -en "\x02"; echo -n 'SendRawEmail' | openssl sha256 -hmac $secret_access_key -binary) | base64
```

The following is an example Java implementation that converts an AWS Secret Access Key to an Amazon SES SMTP password\. Before you run the program, put the AWS Secret Access Key of the IAM user into an environment variable called AWS\_SECRET\_ACCESS\_KEY\. The output of the program is the SMTP password\. That password, along with the SMTP username \(which, if you generate the SMTP password programmatically, is the same as the AWS access key ID\) are the user's Amazon SES SMTP credentials\.

```
 1. import javax.crypto.Mac;
 2. import javax.crypto.spec.SecretKeySpec;
 3. import javax.xml.bind.DatatypeConverter;
 4. 
 5. public class SesSmtpCredentialGenerator {
 6.    private static final String KEY_ENV_VARIABLE = "AWS_SECRET_ACCESS_KEY"; // Put your AWS secret access key in this environment variable.
 7.    private static final String MESSAGE = "SendRawEmail"; // Used to generate the HMAC signature. Do not modify.
 8.    private static final byte VERSION =  0x02; // Version number. Do not modify.
 9. 
10.    public static void main(String[] args) {
11. 				
12. 		  // Get the AWS secret access key from environment variable AWS_SECRET_ACCESS_KEY.
13. 		  String key = System.getenv(KEY_ENV_VARIABLE);         	  
14. 		  if (key == null)
15. 		  {
16. 			 System.out.println("Error: Cannot find environment variable AWS_SECRET_ACCESS_KEY.");  
17. 			 System.exit(0);
18. 		  }
19. 				   
20. 		  // Create an HMAC-SHA256 key from the raw bytes of the AWS secret access key.
21. 		  SecretKeySpec secretKey = new SecretKeySpec(key.getBytes(), "HmacSHA256");
22. 
23. 		  try {         	  
24. 				 // Get an HMAC-SHA256 Mac instance and initialize it with the AWS secret access key.
25. 				 Mac mac = Mac.getInstance("HmacSHA256");
26. 				 mac.init(secretKey);
27. 
28. 				 // Compute the HMAC signature on the input data bytes.
29. 				 byte[] rawSignature = mac.doFinal(MESSAGE.getBytes());
30. 
31. 				 // Prepend the version number to the signature.
32. 				 byte[] rawSignatureWithVersion = new byte[rawSignature.length + 1];               
33. 				 byte[] versionArray = {VERSION};                
34. 				 System.arraycopy(versionArray, 0, rawSignatureWithVersion, 0, 1);
35. 				 System.arraycopy(rawSignature, 0, rawSignatureWithVersion, 1, rawSignature.length);
36. 
37. 				 // To get the final SMTP password, convert the HMAC signature to base 64.
38. 				 String smtpPassword = DatatypeConverter.printBase64Binary(rawSignatureWithVersion);       
39. 				 System.out.println(smtpPassword);
40. 		  } 
41. 		  catch (Exception ex) {
42. 				 System.out.println("Error generating SMTP password: " + ex.getMessage());
43. 		  }             
44.    }
45. }
```
