# Providing the Delegate Sender with the Identity Information for Amazon SES Sending Authorization<a name="sending-authorization-identity-owner-tasks-identity"></a>

After you create your sending authorization policy and attach it to your identity, you can provide the delegate sender with the Amazon Resource Name \(ARN\) of the identity\. The delegate sender will pass that ARN to Amazon SES in the email\-sending operation or in the header of the email\. Use the following procedure to find your identity's ARN\.

**To find the ARN of an identity**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Identity Management**, choose either **Domains** or **Email Addresses**\.

1. In the list of identities, choose the identity to which you attached the sending authorization policy\.

1. At the top of the details pane, after **Identity ARN**, you will see the identity's ARN\. It will look similar to *arn:aws:ses:us\-east\-1:123456789012:identity/user@example\.com*\. Copy the entire ARN and give it to your delegate sender\.
