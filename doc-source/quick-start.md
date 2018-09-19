# Amazon SES Quick Start<a name="quick-start"></a>


|  | 
| --- |
| Are you trying to send an email to Amazon\.com to inquire about your account or an order that you placed? If so, see [Contact Us](http://www.amazon.com/gp/help/customer/contact-us/) on the Amazon website\. | 

This procedure leads you through the steps to sign up for AWS, verify your email address, send your first email, consider how you will handle bounces and complaints, and move out of the Amazon Simple Email Service \(Amazon SES\) sandbox\.

Use this procedure if you:
+ Are just experimenting with Amazon SES\.
+ Want to send some test emails without doing any programming\.
+ Want to get set up in as few steps as possible\.

## Step 1: Sign up for AWS<a name="quick-start-sign-up-for-aws"></a>

Before you can use Amazon SES, you need to sign up for AWS\. When you sign up for AWS, your account is automatically signed up for all AWS services\.

For instructions, see [Signing up for AWS](sign-up-for-aws.md)\.

## Step 2: Verify your email address<a name="quick-start-verify-email-addresses"></a>

Before you can send email from your email address through Amazon SES, you need to show Amazon SES that you own the email address by verifying it\.

For instructions, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)\.

## Step 3: Send your first email<a name="quick-start-send-email-to-yourself"></a>

You can send an email simply by using the Amazon SES console\. As a new user, your account is in a test environment called the sandbox, so you can only send email to and from email addresses that you have verified\.

For instructions, see [Send an Email Using the Amazon SES Console](send-an-email-from-console.md)\.

## Step 4: Consider how you will handle bounces and complaints<a name="quick-start-feedback"></a>

Before the next step, you need to think about how you will handle bounces and complaints\. If you are sending to a small number of recipients, your process can be as simple as examining the bounce and complaint feedback that you receive by email, and then removing those recipients from your mailing list\.

## Step 5: Move out of the Amazon SES sandbox<a name="quick-start-request-production-access"></a>

To be able to send emails to unverified email addresses and to raise the number of emails you can send per day and how fast you can send them, your account needs to be moved out of the sandbox\. This process involves opening an SES Sending Limits Increase case in Support Center\.

For more information about the sandbox restrictions and how to apply to move out of the sandbox, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\.

## Next steps<a name="quick-start-next-steps"></a>
+ After you send a few test emails to yourself, use the Amazon SES mailbox simulator for further testing because emails to the mailbox simulator do not count towards your sending quota or your bounce and complaint rates\. For more information on the mailbox simulator, see [Testing Email Sending in Amazon SES](mailbox-simulator.md)\.
+ Monitor your sending activity, such as the number of emails that you have sent and the number that have bounced or received complaints\. For more information, see [Monitoring Your Amazon SES Sending Activity](monitor-sending-activity.md)\.
+ Verify entire domains so that you can send email from any email address in your domain without verifying addresses individually\. For more information, see [Verifying Domains in Amazon SES](verify-domains.md)\.
+ Increase the chance that your emails will be delivered to your recipients' inboxes instead of junk boxes by authenticating your emails\. For more information, see [Authenticating Your Email in Amazon SES](authentication.md)\.


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, visit the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 