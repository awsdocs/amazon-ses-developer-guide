# Providing Information to the Identity Owner for Amazon SES Sending Authorization<a name="sending-authorization-delegate-sender-tasks-information"></a>

As a delegate sender, you must provide the identity owner with your AWS account ID or the Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) user who will send email on behalf of the identity owner\. You can find this information by using the following procedures\.

**To find your AWS account ID**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com](https://console.aws.amazon.com)\.

1. In the navigation menu, choose your name, and then choose **My Account**\.

1. Expand **Account Settings**\. This section displays your AWS account ID\.

**To find the ARN of an IAM user**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. In the list of users, choose the user name\. The **Summary** section displays the ARN\. The ARN resembles the following example: *arn:aws:iam::123456789012:user/John*\.