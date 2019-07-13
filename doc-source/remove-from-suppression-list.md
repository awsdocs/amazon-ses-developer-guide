# Removing an Email Address from the Amazon SES Suppression List<a name="remove-from-suppression-list"></a>

Amazon SES maintains a suppression list of recipient email addresses that have recently caused a hard bounce for any Amazon SES customer\. If you try to send an email through Amazon SES to an address that is on the suppression list, the call to Amazon SES succeeds, but Amazon SES treats the email as a hard bounce instead of attempting to send it\. Like any hard bounce, suppression list bounces count towards your sending quota and your bounce rate\. An email address can remain on the suppression list for up to 14 days\.

The only way you will know if an address is on the suppression list is that you will receive a suppression list bounce when you send to it\. There is no way to query the suppression list in advance\.

**Important**  
As with any email address that hard bounces, you should remove addresses that cause a suppression list bounce from your mailing list unless you are certain that the address is valid\. Suppression list bounces count towards your account's bounce rate\. If your bounce rate gets too high, we might place your account under review or pause your account's ability to send email\. If you remove an address from the suppression list when it is indeed undeliverable, then the next time you or another Amazon SES customer sends an email to that address, it will hard bounce and the address will go back on the suppression list\.

If you are sure that an address on the suppression list is valid, you can remove it from the list by using the following procedure\. Although each AWS region has a separate suppression list, if you remove an address from the suppression list of one region, the address is removed from the suppression list of all regions\.

**To remove an email address from the suppression list**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the Navigation pane, choose **Suppression List Removal**\.

1. In the **Email Address** field, type the email address that you want to remove from the suppression list\.

1. In the **Type characters** field, type the characters that you see in the image above it\.

1. Choose **Submit**\.

After you submit the form, you can fill out the form for another email address\. Suppression list removal requests are processed immediately\.