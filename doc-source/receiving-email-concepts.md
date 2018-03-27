# Amazon SES Email\-Receiving Concepts<a name="receiving-email-concepts"></a>

When you use Amazon SES as your email receiver, you must tell the service what to do with your mail\. The primary method, which gives you fine\-grained control over your mail, is to specify the actions to take based on the recipient\. The other method is to block or allow mail based on the originating IP address\. This topic describes both methods\.

## Recipient\-Based Control<a name="receiving-email-concepts-rules"></a>

The primary way to control your incoming mail is to specify how mail is handled based on its recipient\. For example, if you own *example\.com*, you can specify that mail for *user@example\.com* should bounce, and that all other mail for *example\.com* and its subdomains should be delivered\. The list of recipients you provide is called the *condition*\.

You set up *receipt rules* to specify how to handle the mail when a condition is satisfied\. A receipt rule consists of a condition and an ordered list of actions\. If the recipient to whom the incoming mail is addressed matches a recipient specified in the condition, then Amazon SES performs the actions specified in the rule\. The following actions are available:
+ **S3 action—**Delivers the mail to an Amazon S3 bucket and, optionally, notifies you through Amazon SNS\.
+ **SNS action—**Publishes the mail to an Amazon SNS topic\. 
**Note**  
The SNS action includes a complete copy of the email content in the Amazon SNS notifications\. The other Amazon SNS notifications mentioned here simply notify you of email delivery; they contain information about the email, not the email content itself\.
+ **Lambda action—**Calls your code through a Lambda function and, optionally, notifies you through Amazon SNS\.
+ **Bounce action—**Rejects the email by returning a bounce response to the sender and, optionally, notifies you through Amazon SNS\.
+ **Stop action—**Terminates the evaluation of the receipt rule set and, optionally, notifies you through Amazon SNS\.
+ **Add header action—**Adds a header to the received email\. You typically use this action only in combination with other actions\.
+ **WorkMail action—**Handles the mail with Amazon WorkMail\. You will typically not use this action directly because Amazon WorkMail takes care of the setup\.

Receipt rules are grouped together into *receipt rule sets*\. You can define multiple receipt rule sets for your AWS account, but only one receipt rule set is active at any time\. The following figure shows how receipt rules, receipt rule sets, and actions relate to each other\.

![\[Inbound email overview\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/inbound_overview.png)

## IP Address\-Based Control<a name="receiving-email-concepts-ip-filters"></a>

You can control your mail flow on a broader level by setting up *IP address filters*\. IP address filters are optional and enable you to specify whether to accept or reject mail originating from an IP address or range of IP addresses\. Your IP address filters can include *block lists* \(IP addresses from which you want to block incoming mail\) and *allow lists* \(IP addresses from which you want to always accept mail\)\. IP address filters are useful for blocking spam\. Amazon SES maintains its own block list of IP addresses known to send spam, but you can choose to receive mail from those IP addresses by adding them to your allow list\.

**Note**  
If you want to allow mail that originates from an Amazon EC2 IP address, you must add it to your allow list\. All mail originating from Amazon EC2 is blocked by default\.

## Email\-Receiving Process<a name="receiving-email-process"></a>

When Amazon SES receives an email for your domain, the following events occur:

1. Amazon SES first looks at the IP address of the sender\. Amazon SES allows the mail to pass this stage unless:
   + The IP address is in your block list\.
   + The IP address is in the Amazon SES block list and not on your allow list\.

1. Amazon SES examines your active receipt rule set to determine whether any of your receipt rules contain a condition that matches any of the incoming email's recipients\.

1. If there aren't any matches, Amazon SES rejects the mail\. Otherwise, Amazon SES accepts the mail\. 

1. If Amazon SES accepts the mail, it evaluates your active receipt rule set\. All of the receipt rules that match at least one of the recipient conditions are applied in the order that they are defined, unless an action or a receipt rule explicitly terminates evaluation of the receipt rule set\.

Now that you have an overview of the process, you can get started by going to [Setting Up Email Receiving](receiving-email-setting-up.md)\.