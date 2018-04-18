# Moving Out of the Amazon SES Sandbox<a name="request-production-access"></a>

To help prevent fraud and abuse, and to help protect your reputation as a sender, we apply certain restrictions to new Amazon SES accounts\. 

We place all new accounts in the Amazon SES *sandbox*\. While your account is in the sandbox, you can use all of the features of Amazon SES\. However, when your account is in the sandbox, we apply the following restrictions to your account:
+ You can only send mail **to** verified email addresses and domains, or to [the Amazon SES mailbox simulator](mailbox-simulator.md)\.
+ You can only send mail **from** verified email addresses and domains\.
**Note**  
This restriction applies even when your account is not in the sandbox\.
+ You can send a maximum of 200 messages per 24\-hour period\.
+ You can send a maximum of 1 message per second\.

Complete the procedures in this section to request that your account be removed from the sandbox\.

**To request that your account be removed from the Amazon SES sandbox**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Open an [SES Sending Limits Increase case](https://aws.amazon.com/ses/extendedaccessrequest/) in Support Center\.

1. In the form, complete the following sections:
   + **Region**: Choose the AWS Region that you use to send email using Amazon SES\. Your sandbox status and sending limits are separate for each AWS Region\. For more information, see [Regions and Amazon SES](regions.md)\.
**Note**  
If you use Amazon SES in more than one region, complete a separate form for each region where you want to have your account removed from the sandbox\.
   + **Limit**: Choose **Desired Daily Sending Quota** or **Desired Maximum Send Rate**\. Sending limits are described in [Managing Your Amazon SES Sending Limits](manage-sending-limits.md)\.
**Note**  
Amazon SES users often want to request that their accounts be removed from the sandbox and request a sending limit increase at the same time\. For this reason, both of these requests use the same form\. If you want to have your account removed from the sandbox, but you don't need a sending limit increase, you still need to select one of these options\.
   + **New limit value**: Enter your desired sending quota or sending rate\. Only request the amount you think you'll need\. Remember that you aren't guaranteed to receive the amount you request, and the higher the limit you request, the more justification you need to provide\. 
**Note**  
If you want to have your account removed from the sandbox, but don't want a sending limit increase, specify either a daily sending quota of 200 or a maximum send rate of 1, depending on the value you chose for **Limit**\. These are the limits that Amazon SES applies by default to all accounts in the sandbox\.
   + **Mail Type**: Choose the type of email you plan to send\. If you're sending several types of email, choose the type that applies to the majority of your email sending\.
   + **Website URL**: Enter the URL of your website\. Providing this information helps us better understand the type of content that you plan to send\.
   + **My email sending complies with the AWS Service Terms and AUP**: Choose the option that applies to your use case\.
   + **I only send to recipients who have specifically requested my mail**: Choose the option that applies to your use case\.
   + **I have a process to handle bounces and complaints**: Choose the option that applies to your use case\.
   + **Use Case Description**: Describe how you will use Amazon SES to send email\. To help us process your request more quickly, include answers to the following questions:
     + How will you build or acquire your mailing list?
     + How will you handle bounces and complaints?
     + How can recipients unsubscribe from your mailing list, and how will you respond to those requests?
     + How did you choose the increase amount you specified in this request?

   When you finish, choose **Submit**\. Allow one business day for us to review your request\. When we finish reviewing your request, we'll update your case in Support Center\.

There are two ways to determine whether your account has been moved out of the sandbox:
+ The correspondence in your SES Sending Limits Increase case indicates that your request has been granted\.
+ You can successfully use Amazon SES to send an email message from a verified email address to an unverified address\. If you receive a *MessageRejected* error stating that your email address is not verified, then your account is still in the sandbox\.

Once your account is out of the sandbox, you no longer have to verify the addresses or domains for your recipients\. However, you must still verify all identities that you use as "From" or "Return\-Path" addresses\. 