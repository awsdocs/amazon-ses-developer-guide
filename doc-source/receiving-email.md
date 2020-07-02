# Receiving email with Amazon SES<a name="receiving-email"></a>

Amazon Simple Email Service \(Amazon SES\) is a mail server that can both send and receive mail on your behalf\. When you use Amazon SES to receive your mail, Amazon SES handles underlying mail\-receiving operations, such as:
+ communicating with other mail servers
+ scanning for spam and viruses
+ rejecting mail from untrusted sources
+ accepting mail for recipients in your domain

When you receive email, Amazon SES processes it according to instructions you provide\. For example, Amazon SES can deliver incoming mail to an Amazon S3 bucket, publish it to an Amazon SNS topic, or send it to Amazon WorkMail\. You can also create rules that explicitly block or allow all messages from specific IP address ranges, or that automatically send bounce messages when messages are sent to specific email addresses\.

**Note**  
Amazon SES doesn't include POP or IMAP servers for receiving incoming email\. This means that you can't use an email client such as Microsoft Outlook to receive incoming email\. If you need a solution that can both send and receive email by using an email client, consider using [Amazon WorkMail](https://aws.amazon.com/workmail)\.

Amazon SES only supports email receiving in certain AWS Regions\. For a complete list of Regions where email receiving is supported, see [Amazon Simple Email Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/ses) in the AWS General Reference\.

**Topics**
+ [Amazon SES email receiving concepts](receiving-email-concepts.md)
+ [Getting started receiving email with Amazon SES](receiving-email-getting-started.md)
+ [Setting up Amazon SES email receiving](receiving-email-setting-up.md)
+ [Managing email receiving in Amazon SES](receiving-email-managing.md)