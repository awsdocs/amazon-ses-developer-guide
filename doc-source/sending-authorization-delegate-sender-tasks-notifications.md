# Using Delegate Sender Notifications for Amazon SES Sending Authorization<a name="sending-authorization-delegate-sender-tasks-notifications"></a>

As the delegate sender, bounces and complaints count toward *your* bounce and complaint metrics, not those of the identity owner\. If the bounce or complaint rates for your account get too high, we might place your account under review or pause your account's ability to send email\. For this reason, it's important that you set up notifications and have a process in place to monitor them\. You also need to have a process in place for removing addresses that have bounced or complained from your mailing lists\.

Delegate senders can set up Amazon SES to send notifications when bounce and complaint events occur\. Delegate senders can also set up [event publishing](monitor-using-event-publishing.md) to publish bounce and complaint notifications to Amazon SNS or Kinesis Data Firehose\.

**Note**  
If you set up Amazon SES to send notifications by using Amazon SNS, you're charged standard Amazon SNS rates for the notifications you receive\. For more information, see the [Amazon SNS pricing page](https://aws.amazon.com/sns/pricing)\.

**Topics**
+ [Setting Up an Amazon SES Cross\-Account Identity Notification Configuration](#sending-authorization-delegate-sender-tasks-management-add)
+ [Editing an Amazon SES Cross\-Account Notification Configuration](#sending-authorization-delegate-sender-tasks-management-edit)
+ [Viewing Your Amazon SES Cross\-Account Identity Notifications](#sending-authorization-delegate-sender-tasks-management-list)
+ [Removing an Amazon SES Cross\-Account Identity Notification Configuration](#sending-authorization-delegate-sender-tasks-management-remove)

## Setting Up an Amazon SES Cross\-Account Identity Notification Configuration<a name="sending-authorization-delegate-sender-tasks-management-add"></a>

Before you set up notifications, you need to know the Amazon Resource Name \(ARN\) of the identity that the identity owner has authorized you to use, and for which you want to configure notifications\. For example, the ARN for identity *user@example\.com* would look similar to *arn:aws:ses:us\-east\-1:123456789012:identity/user@example\.com*\. If the identity owner has not given you the identity's ARN, refer them to the procedure in [Providing the Delegate Sender with the Identity Information](sending-authorization-identity-owner-tasks-identity.md)\.

The easiest way to configure notifications is to use the Amazon SES console\. You can also use the [SetIdentityNotificationTopic](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityNotificationTopic.html) API operation, passing the identity's ARN as the `Identity` parameter\.

The following procedure shows you how to set up notifications by using the Amazon SES console\.

**To set up Amazon SNS bounce, complaint, or delivery notifications by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, choose **Cross\-Account Notifications**\.

1. Choose **Add Notification Config**\.

1. On the **Add Notification Configuration** dialog box, for **Identity ARN**, type the ARN of the identity that you want to configure notifications for\. The identity can't belong to the account that you're currently logged in to\.

1. Select the Amazon SNS topics that you want to use for bounces, complaints, or deliveries\. You can also create new Amazon SNS topics for these notifications\.
**Important**  
The Amazon SNS topics that you use for Amazon SES notifications must be in the same AWS Region that you use for sending email using Amazon SES\.

   You can choose to publish bounce, complaint, and delivery notifications to the same Amazon SNS topic or to different Amazon SNS topics\. If you want to use an Amazon SNS topic that you do not own, then the owner of that topic must configure an Amazon SNS access policy that allows your account to call the `SNS:Publish` action on their topic\. For information about how to control access to your Amazon SNS topic through the use of IAM policies, see [Managing Access to Your Amazon SNS Topics](https://docs.aws.amazon.com/sns/latest/dg/AccessPolicyLanguage.html)\.

1. Choose **Save Config** to save your notification configuration\. There may be a brief delay before these changes take effect\.

## Editing an Amazon SES Cross\-Account Notification Configuration<a name="sending-authorization-delegate-sender-tasks-management-edit"></a>

The easiest way to edit notification configurations is to use the Amazon SES console\. If you want to use the Amazon SES API instead, you can use the [SetIdentityNotificationTopic](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityNotificationTopic.html) API operation and pass the identity's ARN as the `Identity` parameter\.

The following procedure shows you how to edit a cross\-account notification configuration by using the Amazon SES console\.

**To edit a cross\-account notification configuration by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Cross\-Account Notifications**\.

   The cross\-account identities for which you have set up notifications are listed in the **Cross\-Account Notifications** details pane\. 

1. Choose the ARN of the identity for which you want to view the notification configuration\.

1. Edit the notification settings, and then choose **Save Config**\.

## Viewing Your Amazon SES Cross\-Account Identity Notifications<a name="sending-authorization-delegate-sender-tasks-management-list"></a>

The easiest way to view your notification configurations is to use the Amazon SES console\. You can also use the [GetIdentityNotificationAttributes](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityNotificationAttributes.html) API operation, passing the identity's ARN as the `Identity` parameter\.

**Note**  
The only cross\-account identities displayed in the cross\-account identity list are the identities for which you have configured notifications by using the procedure described in [Setting Up a Notification Configuration](#sending-authorization-delegate-sender-tasks-management-add)\. 

**To view your cross\-account notification configurations by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Cross\-Account Notifications**\.

   The cross\-account identities for which you have set up notifications are listed in the **Cross\-Account Notifications** details pane\. 

1. Choose the ARN of an identity\.

   The **Edit Configuration Notification** dialog box displays the identity's settings\. 

## Removing an Amazon SES Cross\-Account Identity Notification Configuration<a name="sending-authorization-delegate-sender-tasks-management-remove"></a>

The easiest way to remove a notification configuration is to use the Amazon SES console\. You can also use the [SetIdentityNotificationTopic](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityNotificationTopic.html) API operation, passing the identity's ARN as the `Identity` parameter, and passing null for the `SnsTopic` parameter\. To completely remove the notification configuration, you must perform this operation for each type of notification type \(bounce, complaint, or delivery\) that was set\. 

**Note**  
When you remove a notification configuration, the ARN of the cross\-account identity is removed from the list of cross\-account identity ARNs in the Amazon SES console\. This does not mean that you cannot continue to send for that identity; it just means that you are no longer set up to receive bounce, complaint, or delivery notifications for it\. If you want to re\-enable notifications, you need to repeat the notification setup procedure described in [Setting Up a Notification Configuration](#sending-authorization-delegate-sender-tasks-management-add)\.

The following procedure shows you how to remove a cross\-account notification configuration by using the Amazon SES console\.

**To remove a cross\-account notification configuration by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Cross\-Account Notifications**\.

   The cross\-account identities for which you have set up notifications are listed in the **Cross\-Account Notifications** details pane\. 

1. Choose the box to the left of the cross\-identity that you want to remove, and then choose **Remove**\.

1. In the **Remove Cross\-Account Notification Config** dialog box, choose **Delete Notification config**\.

   The ARN of the cross\-account identity no longer appears in the list of cross\-account identity x\. This does not mean that you cannot send for the identity, just that you no longer have configured notifications for it\.