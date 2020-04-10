# Setting up Email with Amazon SES<a name="send-email-set-up"></a>

To set up email with Amazon Simple Email Service \(Amazon SES\), you need to perform the following tasks:
+ Before you can access Amazon SES or other AWS services, you need to set up an AWS account\. For more information, see [Signing up for AWS](sign-up-for-aws.md)\.
+ Before you send email through Amazon SES, you need to verify that you own the "From" address\. If your account is still in the Amazon SES sandbox, you also need to verify your "To" addresses\. You can verify email addresses or entire domains\. For more information, see [Verifying Identities in Amazon SES](verify-addresses-and-domains.md)\.

The following tasks are optional depending on what you want to do:
+ If you want to access Amazon SES through the Amazon SES API, whether by the Query \(HTTPS\) interface or indirectly through an [AWS SDK](https://aws.amazon.com/tools/), the [AWS Command Line Interface](https://aws.amazon.com/cli/) or the [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/), you need to obtain your AWS access keys\. For more information, see [Getting Your AWS Access Keys](get-aws-keys.md)\.
+ If you want to call the Amazon SES API without handling the low\-level details of the Query interface, you can use an AWS SDK\. For more information, see [Downloading an AWS SDK](download-aws-sdk.md)\.
+ If you want to access Amazon SES through its SMTP interface, you need to obtain your SMTP user name and password\. Your SMTP credentials are different from your AWS credentials\. For more information, see [Getting Your SMTP Credentials for Amazon SES](get-smtp-credentials.md)\.
+ When you first sign up for Amazon SES, your account is in the Amazon SES sandbox\. In the sandbox, you can send emails using the same email\-sending methods as any other Amazon SES user, except that you can only send 200 emails per 24\-hour period at a maximum rate of one email per second, and you can only send emails to addresses you have verified\. To increase your sending quotas and to send email to unverified email addresses, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\.
+ If you want your emails to pass Domain\-based Message Authentication, Reporting and Conformance \(DMARC\) authentication based on Sender Policy Framework \(SPF\), configure your identity to send from a custom MAIL FROM domain as described in [Setting Up a Custom MAIL FROM Domain](mail-from.md)\.


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 