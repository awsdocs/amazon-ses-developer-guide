# Setting up Amazon Simple Email Service<a name="setting-up"></a>

Before you start using Amazon SES, you must complete the following tasks\.

**Topics**
+ [Sign up for AWS](#setting-up-aws-sign-up)
+ [Get your AWS access keys](#get-aws-keys)
+ [Download an AWS SDK](#download-aws-sdk)
+ [Verify your email address](#quick-start-verify-email-addresses)

## Sign up for AWS<a name="setting-up-aws-sign-up"></a>

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html#aws_tasks-that-require-root)\.

## Get your AWS access keys<a name="get-aws-keys"></a>

After you've signed up for AWS, you must obtain your AWS access keys to access Amazon SES through the Amazon SES API, whether by the Query \(HTTPS\) interface directly or indirectly through an [AWS SDK](https://aws.amazon.com/tools/), the [AWS Command Line Interface](https://aws.amazon.com/cli/), or the [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/)\. AWS access keys consist of an access key ID and a secret access key\.

For more information about the types of security keys that you can use in Amazon SES, see [Types of Amazon SES credentials](send-email-concepts-credentials.md)\. For information about obtaining AWS access keys, see [AWS security credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in the *AWS General Reference*\.

## Download an AWS SDK<a name="download-aws-sdk"></a>

To call the Amazon SES API without having to handle low\-level details like assembling raw HTTP requests, you can use an AWS SDK\. The AWS SDKs provide functions and data types that encapsulate the functionality of Amazon SES and other AWS services\. To download an AWS SDK, go to [SDKs](https://aws.amazon.com/tools/#sdk)\. After you download the SDK, [create a shared credentials file](https://docs.aws.amazon.com/credref/latest/refdocs/creds-config-files.html) and specify your AWS access keys\.

## Verify your email address<a name="quick-start-verify-email-addresses"></a>

Before you can send email from your email address through Amazon SES, you must show Amazon SES that you own the email address by verifying it\. For instructions, see [Creating an email address identity](creating-identities.md#verify-email-addresses-procedure)\.