# Sending Your Email with Amazon SES<a name="choose-email-sending-method"></a>

You can send an email with Amazon Simple Email Service \(Amazon SES\) using the Amazon SES console, the Amazon SES Simple Mail Transfer Protocol \(SMTP\) interface, or the Amazon SES API\. You typically use the console to send test emails and manage your sending activity\. To send bulk emails, you use either the SMTP interface or the API\. For information about Amazon SES email pricing, see [Amazon SES Pricing](https://aws.amazon.com/ses/pricing)\.
+ If you want to use an SMTP\-enabled software package, application, or programming language to send email through Amazon SES, or integrate Amazon SES with your existing mail server, use the Amazon SES SMTP interface\. For more information, see [Using the Amazon SES SMTP Interface to Send Email](send-email-smtp.md)\.
+ If you want to call Amazon SES by using raw HTTP requests, use the Amazon SES API\. For more information, see [Using the Amazon SES API to Send Email](send-email-api.md)\.

Before you send emails, see [Setting up Email with Amazon SES](setting-up-email.md)\.

**Important**  
When you send an email to multiple recipients \(recipients are "To", "CC", and "BCC" addresses\) and the call to Amazon SES fails, the entire email is rejected and none of the recipients will receive the intended email\. We therefore recommend that you send an email to one recipient at a time\.


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, visit the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 