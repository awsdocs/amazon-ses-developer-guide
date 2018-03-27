# Signing up for AWS<a name="sign-up-for-aws"></a>

You need to create an AWS account before you can use Amazon SES or other AWS services\. When you create an AWS account, AWS automatically signs up your account for all services\. You are charged only for the services that you use\.

**Note**  
If you will be sending your emails from an Amazon EC2 instance either directly or through AWS Elastic Beanstalk, you can get started with Amazon SES for free\. For more information, see [Amazon SES Pricing](https://aws.amazon.com/ses/pricing/)\. 

When you first sign up for AWS, your Amazon SES sending is in the Amazon SES sandbox\. In the sandbox, you have full access to the Amazon SES API and SMTP interface\. However, the following restrictions are in effect:
+ You can only send emails to the Amazon SES mailbox simulator and to email addresses or domains that you have verified\. For more information, see [Verifying Identities in Amazon SES](verify-addresses-and-domains.md)\.
+ You can send a maximum of 200 messages per 24\-hour period\.
+ You can send a maximum of one message per second\.

For information about moving out of the sandbox, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\.

**To create an AWS account**

1. Go to [https://aws\.amazon\.com/ses](https://aws.amazon.com/ses), and choose *Sign up now*\.

1. Follow the on\-screen instructions\.

**Note**  
Even if your account is out of the Amazon SES sandbox, you still need to verify your "From" address to confirm that you own it\.