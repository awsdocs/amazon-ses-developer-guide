# Using delegate sender notifications for Amazon SES sending authorization<a name="sending-authorization-delegate-sender-tasks-notifications"></a>

As a delegate sender, you're sending emails on behalf of an identity that you don't own, but are authorized to use; however, bounces and complaints still count toward *your* bounce and complaint metrics, not those of the identity owner\.

If the bounce or complaint rates for your account gets too high, your account is at risk of being placed under review or have its ability to send email paused\. For this reason, it's important that you set up notifications and have a process in place to monitor them\. You also need to have a process in place for removing addresses that have bounced or complained from your mailing lists\.

Therefore, as a delegate sender, you can configure Amazon SES to send notifications when bounce and complaint events occur for the emails you send on behalf of any identities that you don't own, but have been authorized to use by the identity owner\. You can also set up [event publishing](monitor-using-event-publishing.md) to publish bounce and complaint notifications to Amazon SNS or Kinesis Data Firehose\.

**Note**  
If you set up Amazon SES to send notifications by using Amazon SNS, you're charged standard Amazon SNS rates for the notifications you receive\. For more information, see the [Amazon SNS pricing page](https://aws.amazon.com/sns/pricing)\.

**Topics**
+ [Cross\-account notifications legacy support](#sending-authorization-delegate-sender-tasks-cross-account-sunsetting)
+ [Editing an Amazon SES legacy cross\-account notification configuration](#sending-authorization-delegate-sender-tasks-management-edit)
+ [Viewing your Amazon SES legacy cross\-account identity notifications](#sending-authorization-delegate-sender-tasks-management-list)
+ [Removing an Amazon SES legacy cross\-account identity notification configuration](#sending-authorization-delegate-sender-tasks-management-remove)
+ [Create a new delegate sender notification](#sending-authorization-delegate-sender-tasks-management-add)

## Cross\-account notifications legacy support<a name="sending-authorization-delegate-sender-tasks-cross-account-sunsetting"></a>

Cross\-account notifications were an Amazon SES classic console concept where you'd associate a topic with an identity you didn't own \(that’s the cross\-account\), in the SES new console, this has been replaced by using configuration sets and verified identities in association with delegate sending where you're allowed to use another’s identity \(after they’ve given you permission\) to send email\. This new method allows the flexibility to configure bounce, complaint, delivery, and other event notifications by two constructs depending if you're the delegate sender or the owner of the verified identity:
+ **Configuration sets** – The delegate sender can set up event publishing in their own configuration set that they can specify when sending email from a verified identity they don't own, but have been authorized to send from by the identity owner through an authorization policy\. Event publishing allows bounce, complaint, delivery, and other event notifications to be published to Amazon CloudWatch, Amazon Kinesis Data Firehose, Amazon Pinpoint, and Amazon SNS\. See [Create event destinations](event-destinations-manage.md)\.
+ **Verified identities** – Besides having the identity owner authorize the delegate sender to use one of their verified identities to send email from, they can also, at the request of the delegate sender, configure feedback notifications on the shared identity to use SNS topics owned by the delegate sender\. Only the delegate sender will get these notifications because they own the SNS topic\. See Step 14 for how to [configure an "SNS topic you don't own"](sending-authorization-identity-owner-tasks-policy.md#configure-sns-topic-you-dont-own) in the authorization policy procedures\.

**Note**  
For compatibility, cross\-account notifications are being supported for legacy cross\-account notifications currently being used in your account\. This support is limited to being able to modify and use any current cross\-accounts you created using the Amazon SES classic console; however, you can no longer create *new* cross\-account notifications\. To create new ones in the Amazon SES new console, use the new methods of delegate sending either with configuration sets using [event publishing](event-destinations-manage.md), or with verified identities [configured with your own SNS topics](sending-authorization-identity-owner-tasks-policy.md#configure-sns-topic-you-dont-own)\.

## Editing an Amazon SES legacy cross\-account notification configuration<a name="sending-authorization-delegate-sender-tasks-management-edit"></a>

We recommend using the Amazon SES console to edit notification configurations\. If you want to use the Amazon SES API instead, you can use the [SetIdentityNotificationTopic](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityNotificationTopic.html) API operation and pass the identity's ARN as the `Identity` parameter\.

The following steps show you how to edit a cross\-account notification configuration by using the Amazon SES console\.

**To edit a cross\-account notification configuration by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Cross\-account notifications**\.

1. In the **Cross\-account identities** pane, choose the identity you want to edit by selecting its checkbox\.

1. Choose **Edit**\.

1. On the **Configure SNS topics** pane, expand any of the feedback topic fields, \(*Bounces, Complaints, or Deliveries*\), and choose either an SNS topic you own, **No SNS topic**, **Create SNS topic**, or **SNS topic you don't own**\.

   1. If you chose **SNS topic you don’t own**, enter the **SNS topic ARN** shared with you by the owner of the topic\.

   1. If you chose **Create SNS topic**, a modal is presented where you enter a name in the **Topic name** field with an option to enter a display name if you intend to receive notifications through AWS SMS\. After choosing **Create topic**, the topic will be available for you to choose in any of the feedback topic fields\.

1. \(Optional\) If you want your topic notification to include the headers from the original email, check the **Include original email headers** box directly underneath the SNS topic name of each feedback type\. This option is only available if you've assigned an Amazon SNS topic to the associated notification type\. For information about the contents of the original email headers, see the `mail` object in [Notification contents](notification-contents.md)\.

1. Choose **Save changes**\. The changes you made to your notification settings might take a few minutes to take effect\.

## Viewing your Amazon SES legacy cross\-account identity notifications<a name="sending-authorization-delegate-sender-tasks-management-list"></a>

We recommend using the Amazon SES console to view your notification configurations\. You can also use the [GetIdentityNotificationAttributes](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityNotificationAttributes.html) API operation, passing the identity's ARN as the `Identity` parameter\.

**Note**  
The only cross\-account identities displayed in the cross\-account identity list are the identities for which you have configured notifications by using the procedure described in [Create a new delegate sender notification](#sending-authorization-delegate-sender-tasks-management-add)\. 

**To view your cross\-account notification configurations by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Cross\-account notifications**\.

1. In the **Cross\-account identities** pane, the ARNs of any cross\-account identities that you've been authorized to use are listed along with columns to view its SNS topic configuration for bounces, complaints, and deliveries\.

## Removing an Amazon SES legacy cross\-account identity notification configuration<a name="sending-authorization-delegate-sender-tasks-management-remove"></a>

We recommend using the Amazon SES console to remove a notification configuration\. You can also use the [SetIdentityNotificationTopic](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityNotificationTopic.html) API operation, passing the identity's ARN as the `Identity` parameter, and passing null for the `SnsTopic` parameter\. To completely remove the notification configuration, you must perform this operation for each type of notification type \(bounce, complaint, or delivery\) that was set\. 

**Note**  
When you remove a notification configuration, the ARN of the cross\-account identity is removed from the list of cross\-account identity ARNs in the Amazon SES console\. This doesn't mean that you can't continue to send for that identity; it only means that you're no longer setup to receive bounce, complaint, or delivery notifications for it\. If you want to re\-enable notifications, you need to repeat the notification setup procedure described in [Create a new delegate sender notification](#sending-authorization-delegate-sender-tasks-management-add)\.

The following steps show you how to remove a cross\-account notification configuration by using the Amazon SES console\.

**To remove a cross\-account notification configuration by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Cross\-account notifications**\.

1. In the **Cross\-account identities** list, select the checkbox next to the cross\-account identity you want to remove, and choose **Delete**\.

1. In the **Delete cross\-account identity?** confirmation popup, choose **Confirm**\.

## Create a new delegate sender notification<a name="sending-authorization-delegate-sender-tasks-management-add"></a>

As explained previously in [Cross\-account notifications legacy support](#sending-authorization-delegate-sender-tasks-cross-account-sunsetting), you can no longer create *new* cross\-account notifications, but you can use the new methods of delegate sending either with configuration sets using [event publishing](event-destinations-manage.md), or with verified identities [configured with your own SNS topics](sending-authorization-identity-owner-tasks-policy.md#configure-sns-topic-you-dont-own)\.

Procedures are given below for setting up new delegate sending notifications using either method:
+ Event publishing through a configuration set
+ Feedback notifications to SNS topics you own

**To set up event publishing through a configuration set for your delegate sending**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Follow the procedures in [Create event destinations](event-destinations-manage.md)\.

1. After you've set up event publishing in your configuration set, specify the name of the configuration set when you send email as a delegate sender using the verified identity the identity owner authorized you to send from\. See [Sending emails for the identity owner](sending-authorization-delegate-sender-tasks-email.md)\.

**To set up feedback notifications to SNS topics you own for your delegate sending**

1. After you've decided which of your SNS topics you'd like to use for feedback notifications, follow the procedures [to find your SNS topic ARN](sending-authorization-delegate-sender-tasks-information.md#find-sns-topic-arn) and copy the full ARN and share it with your identity owner\.

1. Ask your identity owner to configure your SNS topics for feedback notifications on the shared identity they authorized you to send from\. \(Your identity owner will need to follow the procedures given for [configuring SNS topics](sending-authorization-identity-owner-tasks-policy.md#configure-sns-topic-you-dont-own) in the authorization policy procedures\.\)