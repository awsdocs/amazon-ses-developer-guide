# Receiving email with Amazon SES<a name="receiving-email"></a>

Amazon Simple Email Service \(Amazon SES\) is a mail server that can both send and receive mail on your behalf\. When you use Amazon SES to receive your mail, Amazon SES handles underlying mail\-receiving operations, such as:
+ communicating with other mail servers
+ scanning for spam and viruses
+ rejecting mail from untrusted sources
+ accepting mail for recipients in your domain

When you receive email, Amazon SES processes it according to instructions you provide\. For example, Amazon SES can deliver incoming mail to an Amazon S3 bucket, publish it to an Amazon SNS topic, or send it to Amazon WorkMail\. You can also create rules that explicitly block or allow all messages from specific IP address ranges, or that automatically send bounce messages when messages are sent to specific email addresses\.

**Note**  
Amazon SES only supports email receiving in certain AWS Regions\. For a complete list of Regions where email receiving is supported, see [Amazon Simple Email Service Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/ses) in the AWS General Reference\.

The following sections contain the information you need to understand, set up, and use Amazon SES to receive your mail\.
+ [Email\-Receiving Concepts](receiving-email-concepts.md)
+ [Getting Started Receiving Email](receiving-email-getting-started.md)
+ [Setting Up Email Receiving](receiving-email-setting-up.md)
+ [Managing Email Receiving](receiving-email-managing.md)


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 