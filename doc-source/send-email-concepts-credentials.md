# Types of Amazon SES credentials<a name="send-email-concepts-credentials"></a>

To interact with Amazon Simple Email Service \(Amazon SES\), you use security credentials to verify who you are and whether you have permission to interact with Amazon SES\. There are different types of credentials, and the credentials you use depend on what you want to do\. For example, you use AWS access keys when you send an email using the Amazon SES API, and SMTP credentials when you send an email using the Amazon SES SMTP interface\.

The following table lists the types of credentials you might use with Amazon SES, depending on what you are doing\.


****  

| If you want to access the\.\.\. | Use these credentials | What the credentials consist of | How to get the credentials | 
| --- | --- | --- | --- | 
| Amazon SES API\(You might access the Amazon SES API directly, or indirectly through an AWS SDK, the AWS Command Line Interface, or the AWS Tools for Windows PowerShell\.\) | AWS access keys | Access key ID and secret access key | See [Access Keys](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) in the *AWS General Reference*\.  For security best practice, use AWS Identity and Access Management \(IAM\) user access keys instead of AWS account access keys\. Your AWS account credentials grant full access to all your AWS resources, so you should store them in a safe place and instead use IAM user credentials for day\-to\-day interaction with AWS\. For more information, see [Root Account Credentials vs\. IAM User Credentials](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html) in the *AWS General Reference*\.   | 
| Amazon SES SMTP interface | SMTP credentials | User name and password | See [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\.  Although your Amazon SES SMTP credentials are different than your AWS access keys and IAM user access keys, Amazon SES SMTP credentials are actually a type of IAM credentials\. An IAM user can create Amazon SES SMTP credentials, but the root account owner must ensure that the IAM user's policy gives them permission to access the following IAM actions: "iam:ListUsers", "iam:CreateUser", "iam:CreateAccessKey", and "iam:PutUserPolicy"\.  | 
| Amazon SES console | IAM user name and passwordOREmail address and password | IAM user name and passwordOREmail address and password | See [IAM User Name and Password](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#iam-user-name-and-password) and [Email Address and Password](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#email-and-password-for-your-AWS-account) of the *AWS General Reference*\.  For security best practice, use an IAM user name and password instead of an email address and password\. The email address and password combination are for your AWS account, so you should store them in a safe place instead of using them for day\-to\-day interaction with AWS\. For more information, see [Root Account Credentials vs\. IAM User Credentials](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html) in the *AWS General Reference*\.   | 

For more information about different types of AWS security credentials \(except for SMTP credentials, which are used only for Amazon SES\), see [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in the *AWS General Reference*\.


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 