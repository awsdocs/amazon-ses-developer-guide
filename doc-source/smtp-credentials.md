# Obtaining your Amazon SES SMTP credentials<a name="smtp-credentials"></a>

You need an Amazon SES SMTP user name and password to access the Amazon SES SMTP interface\.

The credentials that you use to send email through the Amazon SES SMTP interface are unique to each AWS Region\. If you use the Amazon SES SMTP interface to send email in more than one Region, you have to [generate a set of SMTP credentials](#smtp-credentials) for each Region\.

**Note**  
If you created SMTP credentials before January 10, 2019, your SMTP credentials were created using an older version of the AWS Signature\. For security purposes, you should delete credentials that you created before this date, and replace them with newer credentials\. You can [delete older credentials by using the IAM console](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_manage.html#id_users_deleting)\.

**Important**  
Your SMTP password is different from your AWS secret access key\. For more information about credentials, see [Types of Amazon SES credentials](send-email-concepts-credentials.md)\.

## Obtaining Amazon SES SMTP credentials using the Amazon SES console<a name="smtp-credentials-console"></a>

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

You can view a list of existing SMTP credentials that you've created using this procedure by going to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. In the navigation pane, under **Access management**, choose **Users**\. Use the search bar to find all users that contain the text "ses\-smtp\-user"\.

You can also use the IAM console to delete existing SMTP users\. To learn more about deleting users, see [https://docs\.aws\.amazon\.com/IAM/latest/UserGuide/Managing IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_manage.html) in the *IAM Getting Started Guide*\.

If you want to rotate your SMTP credentials, complete the procedure above to generate a new set of SMTP credentials\. Then, test the new credentials to ensure that they work as expected\. Finally, delete the IAM user associated with the old SMTP credentials in the IAM console\. For more information about deleting users in IAM, see [Managing users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_manage.html#id_users_deleting) in the *IAM User Guide*\.

## Obtaining Amazon SES SMTP credentials by converting existing AWS credentials<a name="smtp-credentials-convert"></a>

If you have an IAM user that you set up using the IAM interface, you can derive the user's Amazon SES SMTP credentials from their AWS credentials\.

**Important**  
Don't use temporary AWS credentials to derive SMTP credentials\. The Amazon SES SMTP interface doesn't support SMTP credentials that have been generated from temporary security credentials\. 

To enable the IAM user to send email using the Amazon SES SMTP interface, you need to do the following two steps:
+ Derive the user's SMTP credentials from their AWS credentials using the algorithm provided in this section\. Because you are starting from AWS credentials, the SMTP user name is the same as the AWS access key ID, so you just need to generate the SMTP password\.
+ Apply the following policy to the IAM user:

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": "ses:SendRawEmail",
              "Resource": "*"
          }
      ]
  }
  ```

  For more information about using Amazon SES with IAM, see [Controlling access to Amazon SES](control-user-access.md)\.

**Note**  
Although you can generate Amazon SES SMTP credentials for any IAM user, we recommend that you create a separate IAM user when you generate your SMTP credentials\. For information about why it is good practice to create users for specific purposes, go to [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAMBestPractices.html)\.

The following pseudocode shows an algorithm that converts an AWS Secret Access Key to an Amazon SES SMTP password\.

```
 1. // Modify this variable to include your AWS Secret Access Key
 2. key = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY";
 3.             
 4. // Modify this variable to refer to the AWS Region that you want to use to send email.
 5. region = "us-west-2";
 6.             
 7. // The values of the following variables should always stay the same.
 8. date = "11111111";
 9. service = "ses";
10. terminal = "aws4_request";
11. message = "SendRawEmail";
12. version = 0x04;
13. 
14. kDate = HmacSha256(date, "AWS4" + key);
15. kRegion = HmacSha256(region, kDate);
16. kService = HmacSha256(service, kRegion);
17. kTerminal = HmacSha256(terminal, kService);
18. kMessage = HmacSha256(message, kTerminal);
19. signatureAndVersion = Concatenate(version, kMessage);
20. smtpPassword = Base64(signatureAndVersion);
```

Some programming languages include libraries that you can use to convert an IAM secret access key into an SMTP password\. This section includes a code example that you can use to convert an AWS Secret Access Key to an Amazon SES SMTP password using Python\.

------
#### [ Python ]

```
#!/usr/bin/env python3

import hmac
import hashlib
import base64
import argparse

SMTP_REGIONS = [
    'us-east-2',       # US East (Ohio)
    'us-east-1',       # US East (N. Virginia)
    'us-west-2',       # US West (Oregon)
    'ap-south-1',      # Asia Pacific (Mumbai)
    'ap-northeast-2',  # Asia Pacific (Seoul)
    'ap-southeast-1',  # Asia Pacific (Singapore)
    'ap-southeast-2',  # Asia Pacific (Sydney)
    'ap-northeast-1',  # Asia Pacific (Tokyo)
    'ca-central-1',    # Canada (Central)
    'eu-central-1',    # Europe (Frankfurt)
    'eu-west-1',       # Europe (Ireland)
    'eu-west-2',       # Europe (London)
    'sa-east-1',       # South America (Sao Paulo)
    'us-gov-west-1',   # AWS GovCloud (US)
]

# These values are required to calculate the signature. Do not change them.
DATE = "11111111"
SERVICE = "ses"
MESSAGE = "SendRawEmail"
TERMINAL = "aws4_request"
VERSION = 0x04


def sign(key, msg):
    return hmac.new(key, msg.encode('utf-8'), hashlib.sha256).digest()


def calculate_key(secret_access_key, region):
    if region not in SMTP_REGIONS:
        raise ValueError(f"The {region} Region doesn't have an SMTP endpoint.")

    signature = sign(("AWS4" + secret_access_key).encode('utf-8'), DATE)
    signature = sign(signature, region)
    signature = sign(signature, SERVICE)
    signature = sign(signature, TERMINAL)
    signature = sign(signature, MESSAGE)
    signature_and_version = bytes([VERSION]) + signature
    smtp_password = base64.b64encode(signature_and_version)
    return smtp_password.decode('utf-8')


def main():
    parser = argparse.ArgumentParser(
        description='Convert a Secret Access Key for an IAM user to an SMTP password.')
    parser.add_argument(
        'secret', help='The Secret Access Key to convert.')
    parser.add_argument(
        'region',
        help='The AWS Region where the SMTP password will be used.',
        choices=SMTP_REGIONS)
    args = parser.parse_args()
    print(calculate_key(args.secret, args.region))


if __name__ == '__main__':
    main()
```

To obtain your SMTP password by using this script, save the preceding code as `smtp_credentials_generate.py`\. Then, at the command line, execute the following command:

```
python path/to/smtp_credentials_generate.py wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY us-east-1
```

In the preceding command, do the following:
+ Replace *path/to/* with the path to the location where you saved `smtp_credentials_generate.py`\.
+ Replace *wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY* with the Secret Access Key that you want to convert into an SMTP password\.
+ Replace *us\-east\-1* with the AWS Region in which you want to use the SMTP credentials\.

When this script runs successfully, the only output is your SMTP password\.

------

To use this script, first save the preceding code as `smtp_credentials_generate.py`\. Then, at the command line, execute the following command:

```
python path/to/smtp_credentials_generate.py wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY us-east-1
```

In the preceding command, do the following:
+ Replace *path/to/* with the path to the location where you saved `smtp_credentials_generate.py`\.
+ Replace *wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY* with the Secret Access Key that you want to convert into an SMTP password\.
+ Replace *us\-east\-1* with the AWS Region in which you want to use the SMTP credentials\.

When this script runs successfully, the only output is your SMTP password\.