# Moving Out of the Amazon SES Sandbox<a name="request-production-access"></a>

To help prevent fraud and abuse, and to help protect your reputation as a sender, we apply certain restrictions to new Amazon SES accounts\. 

We place all new accounts in the Amazon SES *sandbox*\. While your account is in the sandbox, you can use all of the features of Amazon SES\. However, when your account is in the sandbox, we apply the following restrictions to your account:
+ You can only send mail **to** verified email addresses and domains, or to [the Amazon SES mailbox simulator](mailbox-simulator.md)\.
+ You can only send mail **from** verified email addresses and domains\.
**Note**  
This restriction applies even when your account isn't in the sandbox\.
+ You can send a maximum of 200 messages per 24\-hour period\.
+ You can send a maximum of 1 message per second\.

When your account is out of the sandbox, you can send email to any recipient, regardless of whether the recipient's address or domain is verified\. However, you still have to verify all identities that you use as "From", "Source", "Sender", or "Return\-Path" addresses\.

Complete the procedures in this section to request that your account be removed from the sandbox\.

**To request that your account be removed from the Amazon SES sandbox**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/](https://console.aws.amazon.com/)\.

1. On the **Support** menu, choose **Support Center**, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/console_region_selector.png)

1. On the **My support cases** tab, choose **Create case**\.

1. Under **Create case**, choose **Service limit increase**\.

1. Under **Case classification**, complete the following sections:
   + For **Limit type**, choose **SES Service Limits**\.
   + For **Mail Type**, choose the type of email that you plan to send\. If more than one value applies, choose the option that applies to the majority of the email that you plan to send\.
   + For **Website URL**, enter the URL of your website\. Providing this information helps us better understand the type of content that you plan to send\.
   + For **My email sending complies with the AWS Service Terms and AUP**, choose the option that applies to your use case\.
   + For **I only send to recipients who have specifically requested my mail**, choose the option that applies to your use case\.
   + For **I have a process to handle bounces and complaints**, choose the option that applies to your use case\.

1. Under **Requests**, complete the following sections:
   + For **Region**, choose the AWS Region that your request applies to\.
   + For **Limit**, choose the type of limit increase that you want to request\. You can choose from the following options:
     + **Desired Maximum Send Quota** – Choose this option if you want to request an increase to the number of emails that your account can send per 24\-hour period in the selected Region\. 

       **Desired Maximum Send Rate** – Choose this option if you want to request an increase to the number of emails that your account can send per second in the selected Region\. 
   + For **New limit value**, enter the limit that you're requesting\. Only request the amount that you think you'll need\. Remember that you aren't guaranteed to receive the amount that you request\.

     If you want to have your account removed from the sandbox, but don't want a sending limit increase, specify either a daily sending quota of 200 or a maximum send rate of 1, depending on the value you chose for **Limit**\. These are the default limits that Amazon SES applies to all accounts in the sandbox\. 
**Note**  
If you want to request that we remove your account from the sandbox in another AWS Region, choose **Add another request**, and then complete the **Region**, **Limit**, and **New limit value** fields for that Region\. Repeat this process for each Region where you want to have your account removed from the sandbox\.

1. Under **Case Description**, for **Use case description**, describe how you plan to use Amazon SES to send email\. To help us process your request, you should answer the following questions:
   + How do you plan to build or acquire your mailing list?
   + How do you plan to handle bounces and complaints?
   + How can recipients opt out of receiving email from you?
   + How did you choose the sending rate or sending quota that you specified in this request?

   If there's additional information that we should consider when evaluating your case, provide that information in this section as well\.

1. Under **Contact options**, for **Preferred contact language**, choose whether you want to receive communications for this case in **English** or **Japanese**\.

1. When you finish, choose **Submit**\.

After we receive your request, we begin the process of evaluating it\. We typically respond to requests for production access within one business day\. However, in some cases, we might contact you to request additional information\.

## Checking the Sandbox Status for Your Account<a name="request-production-access-check-status"></a>

You can use the Amazon SES console to determine if your account is still in the sandbox\.

**To determine if your account is in the sandbox**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Use the Region selector to choose an AWS Region\.

1. In the navigation pane, under **Email Sending**, choose **Sending Statistics**\. 

1. If your account is still in the sandbox in the AWS Region that you selected, you see a banner at the top of the page that resembles the example in the following image\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/sandbox-banner-send-statistics.png)

   If the banner doesn't appear on this page, then your account is no longer in the sandbox in the current Region\.

1. \(Optional\) Repeat steps 2–4 for each AWS Region that you use to send email with Amazon SES\.

You can also determine whether your account is in the sandbox by sending email to an address that you haven't verified\. If your account is in the sandbox, you receive an error message stating that the destination address isn't verified\.