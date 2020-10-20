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

If you want to change your SMTP password, delete your existing SMTP user in the IAM console\. Then, complete the procedures above to generate a new set of SMTP credentials\.

## Obtaining Amazon SES SMTP credentials by converting existing AWS credentials<a name="smtp-credentials-convert"></a>

If you have an IAM user that you set up using the IAM interface, you can derive the user's Amazon SES SMTP credentials from their AWS credentials\.

**Important**  
Don't use temporary AWS credentials to derive SMTP credentials\. The Amazon SES SMTP interface doesn't support SMTP credentials that have been generated from temporary security credentials\. 

To enable the IAM user to send email using the Amazon SES SMTP interface, you need to do the following two steps:
+ Derive the user's SMTP credentials from their AWS credentials using the algorithm provided in this section\. Because you are starting from AWS credentials, the SMTP username is the same as the AWS access key ID, so you just need to generate the SMTP password\.
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

The following pseudocode shows the algorithm that converts an AWS Secret Access Key to an Amazon SES SMTP password\.

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
12. versionInBytes = 0x04;
13. 
14. kDate = HmacSha256(date, "AWS4" + key);
15. kRegion = HmacSha256(region, kDate);
16. kService = HmacSha256(service, kRegion);
17. kTerminal = HmacSha256(terminal, kService);
18. kMessage = HmacSha256(message, kTerminal);
19. signatureAndVersion = Concatenate(versionInBytes, kMessage);
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

# Values that are required to calculate the signature. These values should
# never change.
DATE = "11111111"
SERVICE = "ses"
MESSAGE = "SendRawEmail"
TERMINAL = "aws4_request"
VERSION = 0x04

def sign(key, msg):
    return hmac.new(key, msg.encode('utf-8'), hashlib.sha256).digest()

def calculateKey(secretAccessKey, region):
    signature = sign(("AWS4" + secretAccessKey).encode('utf-8'), DATE)
    signature = sign(signature, region)
    signature = sign(signature, SERVICE)
    signature = sign(signature, TERMINAL)
    signature = sign(signature, MESSAGE)
    signatureAndVersion = bytes([VERSION]) + signature
    smtpPassword = base64.b64encode(signatureAndVersion)
    print(smtpPassword.decode('utf-8'))

def main():
    parser = argparse.ArgumentParser(description='Convert a Secret Access Key for an IAM user to an SMTP password.')
    parser.add_argument('--secret',
            help='The Secret Access Key that you want to convert.',
            required=True,
            action="store")
    parser.add_argument('--region',
            help='The name of the AWS Region that the SMTP password will be used in.',
            required=True,
            choices=[
                'us-east-2',      #US East (Ohio)
                'us-east-1',      #US East (N. Virginia)
                'us-west-2',      #US West (Oregon)
                'ap-south-1',     #Asia Pacific (Mumbai)
                'ap-northeast-2', #Asia Pacific (Seoul)
                'ap-southeast-1', #Asia Pacific (Singapore)
                'ap-southeast-2', #Asia Pacific (Sydney)
                'ap-northeast-1', #Asia Pacific (Tokyo)
                'ca-central-1',   #Canada (Central)
                'eu-central-1',   #Europe (Frankfurt)
                'eu-west-1',      #Europe (Ireland)
                'eu-west-2',      #Europe (London)
                'sa-east-1',      #South America (São Paulo)
                'us-gov-west-1'   #AWS GovCloud (US)
            ],
            action="store")
    args = parser.parse_args()

    calculateKey(args.secret,args.region)

main()
```

------
#### [ Bash ]

```
#!/bin/bash

while test $# -gt 0; do
  case "$1" in
    --secret)
      shift
      KEY=$1
      shift
      ;;
    --region)
      shift
      REGION=$1
      shift
      ;;
    *)
      echo "$1 is not a recognized flag."
      exit 1
      ;;
  esac
done

if [ -z $KEY ]; then
    echo 'Option --secret missing' >&2
    exit 1
fi

if [ -z $REGION ]; then
    echo 'Option --region missing' >&2
    exit 1
fi

DATE="11111111"
SERVICE="ses"
MESSAGE="SendRawEmail"
TERMINAL="aws4_request"
VERSION="04"
KDATE=$(echo -n "${DATE}" | openssl dgst -sha256 -mac hmac -macopt "key:AWS4${KEY}" -binary | xxd -p -c 64)
KREGION=$(echo -n "${REGION}" | openssl dgst -sha256 -mac hmac -macopt "hexkey:${KDATE}" -binary | xxd -p -c 64)
KSERVICE=$(echo -n "${SERVICE}" | openssl dgst -sha256 -mac hmac -macopt "hexkey:${KREGION}" -binary | xxd -p -c 64)
KTERMINAL=$(echo -n "${TERMINAL}" | openssl dgst -sha256 -mac hmac -macopt "hexkey:${KSERVICE}" -binary | xxd -p -c 64)
KMESSAGE=$(echo -n "${MESSAGE}" | openssl dgst -sha256 -mac hmac -macopt "hexkey:${KTERMINAL}" -binary | xxd -p -c 64)
echo -n "${VERSION}${KMESSAGE}" | xxd -r -p | base64;
```

------