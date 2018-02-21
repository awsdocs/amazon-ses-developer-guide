# Moving Out of the Amazon SES Sandbox<a name="request-production-access"></a>

To help protect our customers from fraud and abuse and to help you establish your trustworthiness to ISPs and email recipients, we do not immediately grant unlimited Amazon SES usage to new users\. New users are initially placed in the Amazon SES *sandbox*\. In the sandbox, you have full access to all Amazon SES email\-sending methods and features so that you can test and evaluate the service; however, the following restrictions are in effect:

+ You can only send mail to the Amazon SES mailbox simulator and to verified email addresses and domains\.

+ You can only send mail from verified email addresses and domains\.

+ You can send a maximum of 200 messages per 24\-hour period\.

+ Amazon SES can accept a maximum of one message from your account per second\.

To remove the restriction on recipient addresses and increase your sending limits, you need to open a case in Support Center by using the following instructions\.

**To move out of the Amazon SES sandbox**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Open an [SES Sending Limits Increase case](https://aws.amazon.com/ses/extendedaccessrequest/) in Support Center\. You can also access this link using the **Dedicated IPs** page in the Amazon SES console\.

1. In the form, provide the following information:

   + **Region** – Choose the AWS Region for which you are requesting a sending limit increase\. Note that your Amazon SES sandbox status and sending limits are separate for each AWS Region\. For more information, see [Regions and Amazon SES](regions.md)\.

   + **Limit** – Choose **Desired Daily Sending Quota** or **Desired Maximum Send Rate**\. Sending limits are described in [Managing Your Amazon SES Sending Limits](manage-sending-limits.md)\.
**Note**  
The rate at which Amazon SES accepts your messages might be less than the maximum send rate\.

   + **New limit value** – Enter the amount you are requesting\. Only request the amount you think you'll need\. Remember that you are not guaranteed to receive the amount you request, and the higher the limit you request, the more justification you need to be considered for that amount\. 

   + **Mail Type** – Choose the type of email you plan to send using your dedicated IP address\. If multiple values apply, choose the type that will make up the majority of your email sending\.

   + **Website URL** – Type the URL of your website\. Providing this information will help us better understand the type of content you plan to send\.

   + **My email sending complies with the AWS Service Terms and AUP** – Choose the option that applies to your use case\.

   + **I only send to recipients who have specifically requested my mail** – Choose the option that applies to your use case\.

   + **I have a process to handle bounces and complaints** – Choose the option that applies to your use case\.

   + **Use Case Description** – Describe the ways in which you will use Amazon SES to send email\. To help us process your request more quickly, please answer the following questions:

     + How will you build or acquire your mailing list?

     + How will you handle bounces and complaints?

     + How can recipients unsubscribe from your mailing list, and how will you respond to those requests?

     + How did you choose the increase amount you specified in this request?

   When you finish, choose **Submit**\. We will respond to the case after reviewing your request\. Please allow one business day for processing\.

The following are three ways to determine whether your account has been moved out of the sandbox:

+ The correspondence in your SES Sending Limits Increase case indicates that your request has been granted\.

+ You can successfully use Amazon SES to send an email message from your *verified* email address to an unverified address that you own\. If you receive a *MessageRejected* error instead, stating that your email address is not verified, then you are still in the sandbox\.

+ The Amazon SES console shows that your sending quota is higher than 200 messages per 24\-hour period\. To learn more, see [Monitoring Your Amazon SES Sending Limits](monitor-sending-limits.md)\.

Once you are out of the sandbox, you no longer have to verify "To" addresses or domains; however, you must still verify any additional "From" or "Return\-Path" addresses or domains\. Over time, Amazon SES will gradually increase your sending limits, or you can open another SES Sending Limits Increase case if the gradual increase does not meet your needs\. For more information, see [Managing Your Amazon SES Sending Limits](manage-sending-limits.md)\.