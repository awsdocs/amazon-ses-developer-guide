# Signing up for AWS<a name="sign-up-for-aws"></a>

You have to create an AWS account before you can use Amazon SES or other AWS services\.

**To create an AWS account**

1. In a web browser, go to [https://aws\.amazon\.com/ses](https://aws.amazon.com/ses)\.

1. Choose **Create an AWS Account**\.

1. Follow the on\-screen instructions\.

## Next steps<a name="w5aac11c21c13b7"></a>

After you create your AWS account, you can start setting up Amazon SES\.
+ To start sending email with Amazon SES, you first have to [verify an identity](verify-addresses-and-domains.md)\. An *identity* is an email address or domain that your email is sent from\. 
+ To interact with Amazon SES, you need to [obtain the IAM credentials](get-aws-keys.md) for your account\.
+ When you first start using Amazon SES, your account is in the Amazon SES sandbox\. In the sandbox, you have full access to the Amazon SES API and SMTP interface\. However, the following restrictions are in effect:
  + You can only send email to the [Amazon SES mailbox simulator](send-email-simulator.md), and to [verified email identities](verify-addresses-and-domains.md)\.
  + You can send a maximum of 200 messages per 24\-hour period\.
  + You can send a maximum of one message per second\.

  For information about moving out of the sandbox, see [Moving out of the Amazon SES sandbox](request-production-access.md)\.
