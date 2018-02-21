# Amazon SES Notifications Through Email<a name="notifications-via-email"></a>

Amazon SES can notify you of your bounces and complaints through email using a process called *email feedback forwarding* or through [Amazon Simple Notification Service \(Amazon SNS\)](https://aws.amazon.com/sns)\. This topic is about receiving notifications by email, which is the default setting\. For information about setting up notifications through Amazon SNS, see [Amazon SES Notifications Through Amazon SNS](notifications-via-sns.md)\. Unlike bounce and complaint notifications, delivery notifications are available only through Amazon SNS\.

**Important**  
For several important points about notifications, see [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\.

The following sections describe how to receive bounce and complaint notifications through email:

+ To enable bounce and complaint notifications by email, see [Enabling Email Feedback Forwarding](#notifications-via-email-enabling)\.

+ To disable bounce and complaint notifications by email, see [Disabling Email Feedback Forwarding](#notifications-via-email-disabling)\.

+ To learn the email address to which bounce and complaint notifications are sent, see [Email Feedback Forwarding Destination](#notifications-via-email-destination)\.

## Enabling Email Feedback Forwarding<a name="notifications-via-email-enabling"></a>

Email feedback forwarding is enabled by default\. If you previously disabled it, you can enable it by following the procedures in this section\.

**To enable bounce and complaint forwarding through email using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Email Addresses** if you want to configure bounce and complaint notifications for an email address, or choose **Domains** if you want to configure bounce and complaint notifications for a domain\.

1. In the list of verified senders, choose the email address or domain for which you want to configure bounce and complaint notifications\.

1. In the details pane, expand the **Notifications** section\.

1. Choose **Edit Configuration**\.

1. Under **Email Feedback Forwarding**, choose **Enabled**\.
**Note**  
Changes made to your settings on this page might take a few minutes to take effect\.

You can also enable bounce and complaint notifications through email by using the [ SetIdentityFeedbackForwardingEnabled](http://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityFeedbackForwardingEnabled.html) API operation\.

## Disabling Email Feedback Forwarding<a name="notifications-via-email-disabling"></a>

You must receive bounce and complaint notifications through either Amazon SNS or email feedback forwarding, so you can disable email feedback forwarding only if you select an Amazon SNS topic for both bounce and complaint notifications\. If you selected an Amazon SNS topic for bounces and complaints, you can disable email feedback forwarding by using the following procedure\.

**To disable bounce and complaint forwarding through email using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Email Addresses** if you want to configure bounce and complaint notifications for an email address, or choose **Domains** if you want to configure bounce and complaint notifications for a domain\.

1. In the list of verified senders, choose the email address or domain for which you want to configure bounce and complaint notifications\.

1. In the details pane, expand the **Notifications** section\.

1. Choose **Edit Configuration**\.

1. In the Edit Notification Configuration dialog box, ensure that you have selected an Amazon SNS topic for both bounces and complaints\. Otherwise, you will not be able to disable email feedback forwarding in the next step\.

1. Under **Email Feedback Forwarding**, choose **Disabled**\.

1. Choose **Save Config** to save your notification configuration\.
**Note**  
Changes made to your settings on this page might take a few minutes to take effect\.

You can also disable bounce and complaint notifications through email by using the [ SetIdentityFeedbackForwardingEnabled](http://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityFeedbackForwardingEnabled.html) API operation\. 

## Email Feedback Forwarding Destination<a name="notifications-via-email-destination"></a>

When you receive notifications by email, Amazon SES rewrites the From header and sends the notification to you\. The address to which Amazon SES forwards the notification depends on how you sent the original message\.

If you used the SMTP interface to send the message, then notifications go to the address specified in the MAIL FROM command, which overrides any Return\-Path headers specified in the SMTP DATA\.

If you used the `SendEmail` API operation to send the message, then the notifications are delivered as follows:

+ If you specified the optional `ReturnPath` parameter of `SendEmail`, then notifications go to that address\.

+ Otherwise, notifications go to the address specified in the required `Source` parameter of `SendEmail`, which populates the From header of the message\.

If you used the `SendRawEmail` API operation to send the message, then the notifications are delivered as follows:

+ If you specified the optional `Source` parameter of `SendRawEmail`, then notifications go to that address, overriding any Return\-Path headers specified in the raw message\.

+ Otherwise, if the Return\-Path header was specified in the raw message, then notifications go to that address\.

+ Otherwise, notifications go to the address in the From header of the raw message\.

**Important**  
Regardless of whether you use the SMTP interface, `SendEmail` API, or `SendRawEmail` API, Amazon SES overwrites any Return\-Path headers that you provide\.