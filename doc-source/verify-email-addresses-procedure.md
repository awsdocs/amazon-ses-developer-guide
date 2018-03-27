# Verifying an Email Address<a name="verify-email-addresses-procedure"></a>

You can verify email addresses by using the Amazon SES console or the [VerifyEmailIdentity](http://docs.aws.amazon.com/ses/latest/APIReference/API_VerifyEmailIdentity.html) API operation\.

## Verifying an Email Address Using the Amazon SES Console<a name="verify-email-addresses-procedure-console"></a>

Complete the procedure in this section to verify an email address using the Amazon SES console\.

**To verify an email address using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the console, use the region selector to choose the AWS Region where want to verify the email address, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/verify-email-address-region.png)
**Note**  
To verify an email address for use in more than one region, repeat the procedure in this section for each region\.

1. In the navigation pane, under **Identity Management**, choose **Email Addresses**\. 

1. Choose **Verify a New Email Address**\.

1. In the **Verify a New Email Address** dialog box, type your email address in the **Email Address** field, and then choose **Verify This Email Address**\.

1. Check the inbox for the email address that you're verifying\. You'll receive a message with the following subject line: "Amazon Web Services \- Email Address Verification Request in region `RegionName`," where `RegionName` is the name of the AWS Region you selected in step 2\.

   Click the link in the message\.
**Note**  
The link in the verification message expires 24 hours after the message was sent\. If 24 hours have passed since you received the verification email, repeat steps 1–5 to receive a verification email with a valid link\.

1. In the Amazon SES console, under **Identity Management**, choose **Email Addresses**\. In the list of email addresses, locate the email address you're verifying\. If the email address was verified, the value in the **Status** column is "verified"\.

## Verify an Email Address Using the Amazon SES API<a name="verify-email-addresses-api-procedure"></a>

Use the [VerifyEmailIdentity](http://docs.aws.amazon.com/ses/latest/APIReference/API_VerifyEmailIdentity.html) API operation to create a new email identity\. When you execute this operation, a verification email is sent to the specified address\.

To verify an email address using the AWS CLI, type the following command at the command line: aws ses verify\-email\-identity \-\-email\-address *sender@example\.com*

In the preceding command, replace *sender@example\.com* with the email address that you want to verify\.

For a script that can be used to verify several email identities in a single operation, see [Verify Multiple Email Addresses](sample-code-bulk-verify.md)\.

## Troubleshoot Email Address Verification<a name="verify-email-addresses-troubleshooting"></a>

If you attempted to verify an email address, but didn't receive a verification email from AWS, try the following troubleshooting steps:
+ Check the Junk Mail folder in your email client\.
+ Ensure that your email client isn't applying rules that automatically move certain messages to a folder other than your inbox\.
+ In your email client, add no\-reply\-aws@amazon\.com to your address book or Safe Senders list\. You can also ask your system administrator to whitelist incoming email from no\-reply\-aws@amazon\.com\.
+ With an email address that uses a different email service provider \(such as a personal email address\), send a message to the address you want to verify\. Ensure that the address you want to verify receives the message\. This step is especially important if you recently set up your own domain\. Occasionally, new domains might not be correctly configured to receive incoming email\.

  Alternatively, try to verify an email address that you know is able to receive email, such as a personal email address\. If you receive the verification email at your personal address, it might indicate that there is an issue on the other domain\.

  If these tests show email isn't being received at the address you attempted to verify, consult your system administrator or email service provider for further assistance\.