# Providing information to the identity owner for Amazon SES sending authorization<a name="sending-authorization-delegate-sender-tasks-information"></a>

As a delegate sender, you must provide the identity owner with either your AWS account ID or your IAM user Amazon Resource Name \(ARN\) since you will be sending email on behalf of the identity owner\. The identity owner needs your account information so he can create a policy that grants you permission to send from one of his verified identities\.

If you want to use your own SNS topics, you can request that your identity owner configure feedback notifications for bounces, complaints, or deliveries to be sent to one or more of your SNS topics\. Do do this, you’ll need to share your SNS topic ARN with your identity owner so that he can configure your SNS topic in the verified identity he's authorizing you to send from\.

The following procedures explain how to find your account information and SNS topic ARNs to share with your identity owner\.

**To find your AWS account ID**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com](https://console.aws.amazon.com)\.

1. In the upper right\-hand corner of the console, expand your name/account number, and choose **My Account** in the dropdown\.

1. The Account settings page will open and display all of your account information including your AWS account ID\.

**To find your IAM user ARN**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. In the list of users, choose the user name\. The **Summary** section displays the IAM user ARN\. The ARN resembles the following example: *arn:aws:iam::123456789012:user/John*\.

**To find your SNS topic ARN**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. In the navigation pane, choose **Topics**\.

1. In the list of topics, the SNS topic ARNs are displayed in the **ARN** column\. The ARN resembles the following example: *arn:aws:sns:us\-east\-1:444455556666:my\-sns\-topic*\.