# Creating a policy for Amazon SES sending authorization<a name="sending-authorization-identity-owner-tasks-policy"></a>

To authorize a delegate sender to send emails using an email address or domain \(an *identity*\) that you own, you create a sending authorization policy, and then attach that policy to the identity\. An identity can have zero, one, or many policies\. However, a single policy can only be associated with a single identity\.

You can create a sending authorization policy in the following ways:
+ **By using the policy generator** – You can create a simple policy by using the policy generator in the Amazon SES console\. In addition to specifying who can send the emails, you can constrain the email\-sending with conditions based on the time and date range in which emails can be sent, the "From" address, the "From" display name, the address to which bounces and complaints are sent, the recipient addresses, and the source IP\. You might also want to use the policy generator to create the structure of a simple policy and then customize it later by editing the policy\.
+ **By creating a custom policy** – If you want to include more advanced conditions or use an AWS service as the principal, you can create a custom policy and attach it to the identity by using the Amazon SES console or the Amazon SES API\.

This topic describes both methods\.

**Note**  
Sending authorization policies that you attach to email address identities take precedence over policies that you attach to their corresponding domain identities\. For example, if you create a policy for *example\.com* that disallows a delegate sender, and you create a policy for *sender@example\.com* that allows the delegate sender, then the delegate sender can send email from *sender@example\.com*, but not from any other address on the *example\.com* domain\.  
If you create a policy for *example\.com* that allows a delegate sender, and you create a policy for *sender@example\.com* that disallows the delegate sender, then the delegate sender can send email from any address on the *example\.com* domain, except for *sender@example\.com*\.

## Creating a policy by using the policy generator<a name="sending-authorization-identity-owner-tasks-identity-policy-generator"></a>

You can use the policy generator to create a simple authorization policy by following these steps\.

**To create a policy by using the policy generator**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the **Identities** container on the **Verified identities** screen, select the verified identity you wish to authorize for the delegate sender to send on your behalf\.

1. In the details screen of the verified identity you selected in the previous step, choose the **Authorization** tab\.

1. In the **Sending authorization policies** pane, choose **Create policy** in the lower right corner and select **Use policy generator** from the dropdown\.

1. In the **Create statement** pane, choose **Allow** in the **Effect** field\. \(If you want to create a policy to restrict your delegate sender, choose **Deny** instead\.\)

1. In the **Principals** field, enter the *AWS account ID* or *IAM user or role ARN* that your delegate sender shared with you to authorize them to send email on behalf of your account for this identity, then choose **Add**\. \(If you wish to authorize more than one delegate sender, repeat this step for each one\.\)

1. In the **Actions** field, select the check box for each send type you would like to authorize for your delegate sender\.

1. \(Optional\) Expand **Specify conditions** if you wish to add a qualifying statement to the delegate sender permission\.

   1. Select an operator from the **Operator** dropdown\.

   1. Select a type from the **Key** dropdown\.

   1. Respective to the key type you selected, enter its value in the **Value** field\. \(If you wish to add more conditions, choose **Add new condition** and repeat this step for each additional one\.\)

1. Choose **Save statement**\.

1. \(Optional\) Expand **Create another statement** if you wish to add more statements to your policy and repeat steps 6 \- 10\.

1. Choose **Next** and on the **Customize policy** screen, the **Edit policy details** container has fields where you can change or customize the policy’s **Name** and the **Policy document** itself\.

1. Choose **Next** and on the **Review and apply** screen, the **Overview** container will show the verified identity you’re authorizing for your delegate sender as well as the name of this policy\. In the **Policy document** pane will be the actual policy you just wrote along with any conditions you added \- review the policy and if it looks correct, choose **Apply policy**\. \(If you need to change or correct something, choose **Previous** and work in the **Edit policy details** container\.\) The policy you just created will allow your delegate sender to send on your behalf\. 

1. <a name="configure-sns-topic-you-dont-own"></a>\(Optional\) If your delegate sender also wants to use an SNS topic that they own, to receive feedback notifications when they receive bounces or complaints, or when emails are delivered, you’ll need to configure their SNS topic in this verified identity\. \(Your delegate sender will need to share with you their SNS topic ARN\.\) Select the **Notifications** tab  and select **Edit** in the **Feedback notifications** container:

   1. On the **Configure SNS topics** pane, in any of the feedback fields, \(Bounce, Complaint, or Delivery\), select **SNS topic you don’t own** and enter the **SNS topic ARN** owned and shared with you by your delegate sender\. \(Only your delegate sender will get these notifications because they own the SNS topic \- you, as the identity owner, will not\.\)

   1. \(Optional\) If you want your topic notification to include the headers from the original email, check the **Include original email headers** box directly underneath the SNS topic name of each feedback type\. This option is only available if you've assigned an Amazon SNS topic to the associated notification type\. For information about the contents of the original email headers, see the `mail` object in [Notification contents](notification-contents.md)\.

   1. Choose **Save changes**\. The changes you made to your notification settings might take a few minutes to take effect\.

   1. \(Optional\) Since your delegate sender will be getting Amazon SNS topic notifications for bounces and complaints, you can disable email notifications entirely if you don’t want to receive feedback for this identity’s sends\. To disable email feedback for bounces and complaints, under the **Notifications** tab, in the **Email Feedback Forwarding** container, choose **Edit**, uncheck the **Enabled** box, and choose **Save changes**\. Delivery status notifications will now only be sent to the SNS topics owned by your delegate sender\.

## Creating a custom policy<a name="sending-authorization-identity-owner-tasks-identity-policy-custom"></a>

If you want to create a custom policy and attach it to an identity, you have the following options:
+ **Using the Amazon SES API** – Create a policy in a text editor and then attach the policy to the identity by using the `PutIdentityPolicy` API described in the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\.
+ **Using the Amazon SES console** – Create a policy in a text editor and attach it to an identity by pasting it into the custom policy editor in the Amazon SES console\. The following procedure describes this method\.



**To create a custom policy by using the custom policy editor**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the **Identities** container on the **Verified identities** screen, select the verified identity you wish to authorize for the delegate sender to send on your behalf\.

1. In the details screen of the verified identity you selected in the previous step, choose the **Authorization** tab\.

1. In the **Sending authorization policies** pane, choose **Create policy** in the lower right corner and select **Create custom policy** from the dropdown\.

1. In the **Policy document** pane, paste the text of your policy\.

1. Choose **Apply Policy**\. \(If you ever need to modify your custom policy, just select its check box under the **Authorization** tab, choose **Edit**, and make your changes in the **Policy document** pane followed by **Save changes**\)\.

1. \(Optional\) If your delegate sender also wants to use an SNS topic that they own, to receive feedback notifications when they receive bounces or complaints, or when emails are delivered, you’ll need to configure their SNS topic in this verified identity\. \(Your delegate sender will need to share with you their SNS topic ARN\.\) Select the **Notifications** tab  and select **Edit** in the **Feedback notifications** container:

   1. On the **Configure SNS topics** pane, in any of the feedback fields, \(Bounce, Complaint, or Delivery\), select **SNS topic you don’t own** and enter the **SNS topic ARN** owned and shared with you by your delegate sender\. \(Only your delegate sender will get these notifications because they own the SNS topic \- you, as the identity owner, will not\.\)

   1. \(Optional\) If you want your topic notification to include the headers from the original email, check the **Include original email headers** box directly underneath the SNS topic name of each feedback type\. This option is only available if you've assigned an Amazon SNS topic to the associated notification type\. For information about the contents of the original email headers, see the `mail` object in [Notification contents](notification-contents.md)\.

   1. Choose **Save changes**\. The changes you made to your notification settings might take a few minutes to take effect\.

   1. \(Optional\) Since your delegate sender will be getting Amazon SNS topic notifications for bounces and complaints, you can disable email notifications entirely if you don’t want to receive feedback for this identity’s sends\. To disable email feedback for bounces and complaints, under the **Notifications** tab, in the **Email Feedback Forwarding** container, choose **Edit**, uncheck the **Enabled** box, and choose **Save changes**\. Delivery status notifications will now only be sent to the SNS topics owned by your delegate sender\.
