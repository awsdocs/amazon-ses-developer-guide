# Moving out of the Amazon SES sandbox<a name="request-production-access"></a>

To help prevent fraud and abuse, and to help protect your reputation as a sender, we apply certain restrictions to new Amazon SES accounts\. 

We place all new accounts in the Amazon SES *sandbox*\. While your account is in the sandbox, you can use all of the features of Amazon SES\. However, when your account is in the sandbox, we apply the following restrictions to your account:
+ You can only send mail **to** verified email addresses and domains, or to [the Amazon SES mailbox simulator](send-an-email-from-console.md#send-email-simulator)\.
+ You can send a maximum of 200 messages per 24\-hour period\.
+ You can send a maximum of 1 message per second\.

When your account is out of the sandbox, you can send email to any recipient, regardless of whether the recipient's address or domain is verified\. However, you still have to verify all identities that you use as "From", "Source", "Sender", or "Return\-Path" addresses\.

Complete the procedures in this section to request that your account be removed from the sandbox\.

**Note**  
If you're using Amazon SES to send email from an Amazon EC2 instance, you might also need to request that the throttle be removed from port 25 on your Amazon EC2 instance\. For more information, see [How do I remove the throttle on port 25 from my EC2 instance?](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-port-25-throttle/) in the AWS Knowledge Center\.

**To request that your account be removed from the Amazon SES sandbox using the AWS Management Console**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, choose **Account dashboard**\.

1. In the warning box at the top of the console that says, "Your Amazon SES account is in the sandbox", on the right\-hand side, choose **Request production access**\.

1. In the account details modal, select either the **Marketing** or **Transactional** radio button that best describes the majority of mail you'll be sending\.
   + *Marketing email* \- Sent on a one\-to\-many basis to a targeted list of prospects or customers containing marketing and promotional content such as to make a purchase, download information, etc\.
   + *Transactional email* \- Sent on a one\-to\-one basis unique to each recipient usually triggered by a user action such as a website purchase, a password reset request, etc\.

1. In **Website URL**, enter the URL of your website to help us better understand the kind of content you plan on sending\.

1. In **Use case description**, explain how you plan to use Amazon SES to send email\. To help us process your request, you should answer the following questions:
   + How do you plan to build or acquire your mailing list?
   + How do you plan to handle bounces and complaints?
   + How can recipients opt out of receiving email from you?
   + How did you choose the sending rate or sending quota that you specified in this request?

1. In **Additional contacts**, tell us where you want to receive communications about your account\. This can be a comma\-separated list of up to 4 email addresses\.

1. In **Preferred contact language**, choose whether you want to receive communications in **English** or **Japanese**\.

1. In **Acknowledgement**, check the box that you agree to only send email to individuals who've explicitly requested it and confirm that you have a process in place for handling bounce and complaint notifications\.

1. Choose the **Submit request** button \- a banner will display to confirm your request was submitted and is currently under review\.

Once you submit a review of your account details, you can’t edit your details until the review is complete\. The AWS Support team provides an initial response to your request within 24 hours\.

In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each request carefully\. If we're able to do so, we'll grant your request within this 24\-hour period\. However, if we need to obtain additional information from you, it might take longer to resolve your request\. We might not be able to grant your request if your use case doesn't align with our policies\.

Optionally, you can also submit your request for production access using the AWS CLI\. Submitting your request using the AWS CLI is helpful when you want to request production access for a large number of identities, or when you want to automate the process of setting up Amazon SES\.

**To request that your account be removed from the Amazon SES sandbox using the AWS CLI**

1. **Prerequisite**: you have to install and configure the AWS CLI\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

1. At the command line, enter the following command:

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

   1. Replace *TRANSACTIONAL* with the type of email that you plan to send through Amazon SES\. You can specify either `TRANSACTIONAL` or `PROMOTIONAL`\. If more than one value applies, specify the option that applies to the majority of the email that you plan to send\.

   1. Replace *https://example\.com* with the URL of your website\. Providing this information helps us better understand the type of content that you plan to send\.

   1. Replace *Use case description* with a description of how you plan to use Amazon SES to send email\. To help us process your request, you should answer the following questions:

      1. How do you plan to build or acquire your mailing list?

      1. How do you plan to handle bounces and complaints?

      1. How can recipients opt out of receiving email from you?

      1. How did you choose the sending rate or sending quota that you specified in this request?

   1. Replace *info@example\.com* with the email addresses where you want to receive communications about your account\. This can be a comma\-separated list of up to 4 email addresses\.

   1. Replace *EN* with your preferred language\. You can specify `EN` for English or `JP` for Japanese\.

Once you submit a review of your account details, you can’t edit your details until the review is complete\. The AWS Support team provides an initial response to your request within 24 hours\.

In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each request carefully\. If we're able to do so, we'll grant your request within this 24\-hour period\. However, if we need to obtain additional information from you, it might take longer to resolve your request\. We might not be able to grant your request if your use case doesn't align with our policies\.