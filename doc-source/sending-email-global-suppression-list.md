# Using the Amazon SES Global Suppression List<a name="sending-email-global-suppression-list"></a>

Amazon SES includes a *global suppression list*\. When any Amazon SES customer sends an email that results in a hard bounce, Amazon SES adds the email address that produced the bounce to a global suppression list\. The global suppression list is *global* in the sense that it applies to all Amazon SES customers\. In other words, if a different customer attempts to send an email to an address that's on the global suppression list, Amazon SES accepts the message, but doesn't send it, because the email address is suppressed\. An advantage of the global suppression list is that it applies to all Amazon SES accounts by default—you don't have to perform any additional configuration to use it\. A disadvantage is that you can't query the global suppression list, because it contains email addresses that are associated with other Amazon SES users' accounts\. Also, you can't manually add addresses to the global suppression list, and you can only remove addresses from the global suppression list by using the Amazon SES console\.

Amazon SES also includes an account\-level suppression list\. For more information, see [Using the Account\-Level Suppression List](sending-email-suppression-list.md)\.

## Global Suppression List Considerations<a name="sending-email-global-suppression-list-considerations"></a>

You should consider the following factors when you use the global suppression list:
+ The global suppression list is enabled by default for all Amazon SES accounts\. You can't disable it\.
+ Because Amazon SES applies the global suppression list to all customers, you can't query the global suppression list or add addresses to it manually\.
+ Amazon SES automatically removes email addresses from the global suppression list after 14 days\. At the end of this 14\-day period, Amazon SES automatically removes the address from the global suppression list\. However, if the same address produces another hard bounce, Amazon SES adds it to the global suppression list again\.
+ If you attempt to send a message to an address that's on the global suppression list, Amazon SES accepts the message, but doesn't send it\. Amazon SES generates a bounce notification with a `bounceType` value of `Permanent`, and a `bounceSubType` value of `Suppressed`\. Receiving this type of bounce notification is the only way to know if an address is on the global suppression list\. You can't query the global suppression list\.
+ Amazon SES counts the messages that you send to addresses on the global suppression list toward the bounce rate for your account\.
+ Amazon SES counts the messages that you send to addresses on the global suppression list toward your daily sending quota\.
+ As with any email address that produces a hard bounce, you should remove addresses that cause a suppression list bounce from your mailing list unless you're certain that the address is valid\. Suppression list bounces count towards your account's bounce rate\. If your bounce rate gets too high, we might place your account under review or pause your account's ability to send email\.

## Removing an Address From the Global Suppression List<a name="sending-email-global-suppression-list-remove"></a>

If you're sure that an address on the global suppression list is actually a valid recipient, you can remove it by using the following procedure\. When you remove an address from the global suppression list in one Region, the removal applies to all AWS accounts in all regions\.

**To remove an email address from the global suppression list**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Use the Region selector in the top right corner of the window to choose one of the following AWS Regions: US East \(N\. Virginia\), US West \(Oregon\), or Europe \(Ireland\)\. The specific Region that you choose isn't important, because the suppression list applies equally to all Regions\.
**Note**  
To complete this procedure, you have to use one of the AWS Regions listed in the preceding paragraph\. You can't complete this procedure in other AWS Regions\.

1. In the navigation pane, choose **Suppression List Removal**\.

1. In the **Email Address** field, type the email address that you want to remove from the global suppression list\.

1. In the **Type characters** field, type the characters that you see in the image above it\.

1. Choose **Submit**\.

If the email address that you specify is on the global suppression list, we remove it immediately\.
