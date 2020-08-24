# Moving out of the Amazon SES sandbox<a name="request-production-access"></a>

To help prevent fraud and abuse, and to help protect your reputation as a sender, we apply certain restrictions to new Amazon SES accounts\. 

We place all new accounts in the Amazon SES *sandbox*\. While your account is in the sandbox, you can use all of the features of Amazon SES\. However, when your account is in the sandbox, we apply the following restrictions to your account:
+ You can only send mail **to** verified email addresses and domains, or to [the Amazon SES mailbox simulator](send-email-simulator.md)\.
+ You can only send mail **from** verified email addresses and domains\.
**Note**  
This restriction applies even when your account isn't in the sandbox\.
+ You can send a maximum of 200 messages per 24\-hour period\.
+ You can send a maximum of 1 message per second\.

When your account is out of the sandbox, you can send email to any recipient, regardless of whether the recipient's address or domain is verified\. However, you still have to verify all identities that you use as "From", "Source", "Sender", or "Return\-Path" addresses\.

Complete the procedures in this section to request that your account be removed from the sandbox\.

**Note**  
If you're using Amazon SES to send email from an Amazon EC2 instance, you might also need to request that the throttle be removed from port 25 on your Amazon EC2 instance\. For more information, see [How do I remove the throttle on port 25 from my EC2 instance?](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-port-25-throttle/) in the AWS Knowledge Center\.

**To request that your account be removed from the Amazon SES sandbox**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/](https://console.aws.amazon.com/)\.

1. On the **Support** menu, choose **Support Center**, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/console_region_selector.png)

1. On the **My support cases** tab, choose **Create case**\.

1. Under **Create case**, choose **Service limit increase**\.

1. Under **Case classification**, complete the following sections:
   + For **Limit type**, choose **SES Sending Limits**\.
   + For **Mail Type**, choose the type of email that you plan to send\. If more than one value applies, choose the option that applies to the majority of the email that you plan to send\.
   + For **Website URL**, enter the URL of your website\. Providing this information helps us better understand the type of content that you plan to send\.
   + For **Describe how you will comply with AWS Service Terms and AUP**, explain how you plan to ensure that your email sending complies with both of these documents\.
   + For **Describe how you will only send to recipients who have specifically requested your mail**, explain how you plan to manage your recipients' opt\-in and opt\-out preferences\.
   + For **Describe the process that you will follow when you receive bounce and complaint notifications**, explain what you plan to do if an email results in a bounce or complaint event\.

1. Under **Requests**, complete the following sections:
   + For **Region**, choose the AWS Region that your request applies to\.
   + For **Limit**, choose the type of quota increase that you want to request\. You can choose from the following options:
     + **Desired Maximum Send Quota** – Choose this option if you want to request an increase to the number of emails that your account can send per 24\-hour period in the selected Region\. 

       **Desired Maximum Send Rate** – Choose this option if you want to request an increase to the number of emails that your account can send per second in the selected Region\. 
   + For **New limit value**, enter the quota that you're requesting\. Only request the amount that you think you'll need\. Remember that you aren't guaranteed to receive the amount that you request\.

     If you want to have your account removed from the sandbox, but don't want a sending quota increase, specify either a daily sending quota of 200 or a maximum send rate of 1, depending on the value you chose for **Limit**\. These are the default quotas that Amazon SES applies to all accounts in the sandbox\. 
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

**To submit through the AWS CLI**
   ```
   aws sesv2 put-account-details \
   --mail-type TRANSACTIONAL \
   --website-url https://example.com \
   --use-case-description "A description of how you plan to use Amazon SES to send email." \
   --additional-contact-email-addresses info@example.com \
   --contact-language EN
   ```

The AWS Support team provides an initial response to your request within 24 hours\.

In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each request carefully\. If we're able to do so, we'll grant your request within this 24\-hour period\. However, if we need to obtain additional information from you, it might take longer to resolve your request\.

We might not be able to grant your request if your use case doesn't align with our policies\.

## Checking the sandbox status for your account<a name="request-production-access-check-status"></a>

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
