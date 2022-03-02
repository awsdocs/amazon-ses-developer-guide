# Providing the delegate sender with the identity information for Amazon SES sending authorization<a name="sending-authorization-identity-owner-tasks-identity"></a>

After you create your sending authorization policy and attach it to your identity, you can provide the delegate sender with the Amazon Resource Name \(ARN\) of the identity\. The delegate sender will pass that ARN to Amazon SES in the email\-sending operation or in the header of the email\. To find your identity's ARN, follow these steps\.

**To find the ARN of an identity**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the list of identities, choose the identity to which you attached the sending authorization policy\.

1. In the **Summary** pane, the second column, **Amazon Resource Name \(ARN\)**, will contain the identity's ARN\. It will look similar to *arn:aws:ses:us\-east\-1:123456789012:identity/user@example\.com*\. Copy the entire ARN and give it to your delegate sender\.