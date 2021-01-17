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

**To request that your account be removed from the Amazon SES sandbox using the AWS Management Console**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Email Sending** choose **Sending Statistics**\.

1. For **Your account details**, choose **Edit your account details**, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/production_access_account_details_sandbox.png)

1. In the account details modal, fill out the following account details:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/production_access_account_details_edit.png)
   + For **Enable production access**, choose **Yes** or **No**\. You can only move out of the sandbox by choosing **Yes**\.
   + For **Mail Type**, choose the type of email that you plan to send\. If more than one value applies, choose the option that applies to the majority of the email that you plan to send\.
   + For **Website URL**, enter the URL of your website\. Providing this information helps us better understand the type of content that you plan to send\.
   + For **Use case description**, explain how you plan to use Amazon SES to send email\. To help us process your request, you should answer the following questions:
     + How do you plan to build or acquire your mailing list?
     + How do you plan to handle bounces and complaints?
     + How can recipients opt out of receiving email from you?
     + How did you choose the sending rate or sending quota that you specified in this request?

1. For **Additional contact addresses**, tell us where you want to receive communications about your account\. This can be a comma\-separated list of up to 4 email addresses\.

1. For **Preferred contact language**, choose whether you want to receive communications for this case in **English** or **Japanese**\.

1. When you finish, choose **Submit for review**\.

**Note**  
Once you submit a review of your account details, you can’t edit your details until the review is complete\.

Rather than submit a production access request using the AWS Management Console, you can instead submit the request using the AWS CLI\. Submitting your request using the AWS CLI is helpful when you want to request production access for a large number of identities, or when you want to automate the process of setting up Amazon SES\.

**To request that your account be removed from the Amazon SES sandbox using the AWS CLI**
**Note**  
Before you complete the procedure in this section, you have to install and configure the AWS CLI\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.
+ At the command line, enter the following command:

  ```
  aws sesv2 put-account-details \
  --production-access-enabled \
  --mail-type TRANSACTIONAL \
  --website-url https://example.com \
  --use-case-description "Use case description" \
  --additional-contact-email-addresses info@example.com \
  --contact-language EN
  ```

  In the preceding command, do the following:
  + Replace *TRANSACTIONAL* with the type of email that you plan to send through Amazon SES\. You can specify either `TRANSACTIONAL` or `PROMOTIONAL`\. If more than one value applies, specify the option that applies to the majority of the email that you plan to send\.
  + Replace *https://example\.com* with the URL of your website\. Providing this information helps us better understand the type of content that you plan to send\.
  + Replace *Use case description* with a description of how you plan to use Amazon SES to send email\. To help us process your request, you should answer the following questions:
    + How do you plan to build or acquire your mailing list?
    + How do you plan to handle bounces and complaints?
    + How can recipients opt out of receiving email from you?
    + How did you choose the sending rate or sending quota that you specified in this request?
  + Replace *info@example\.com* with the email addresses where you want to receive communications about your account\. This can be a comma\-separated list of up to 4 email addresses\.
  + Replace *EN* with your preferred language\. You can specify `EN` for English or `JP` for Japanese\.

**Note**  
Once you submit a review of your account details, you can’t edit your details until the review is complete\.

The AWS Support team provides an initial response to your request within 24 hours\.

In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each request carefully\. If we're able to do so, we'll grant your request within this 24\-hour period\. However, if we need to obtain additional information from you, it might take longer to resolve your request\.

We might not be able to grant your request if your use case doesn't align with our policies\.

## Checking the sandbox status for your account<a name="request-production-access-check-status"></a>

You can use the Amazon SES console to determine if your account is still in the sandbox\.

**To determine if your account is in the sandbox**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Email Sending**, choose **Sending Statistics**\. 

1. Under **Your account details**, you will see the status of your account\.

   If your account is still being reviewed, a banner shows that your account is still under review\. Your **Production access** status will still read as **Sandbox**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/production_access_account_details_under_review.png)

   The banner also reflects the status of your account should your request to move out of the sandbox be denied or have failed\.

   If your request to move out of the sandbox has been granted, your **Production access** status is noted as **Enabled**\. This status means then your account is no longer in the sandbox in the current Region\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/production_access_account_details_enabled.png)

You can also determine whether your account is in the sandbox by sending email to an address that you haven't verified\. If your account is in the sandbox, you receive an error message stating that the destination address isn't verified\.