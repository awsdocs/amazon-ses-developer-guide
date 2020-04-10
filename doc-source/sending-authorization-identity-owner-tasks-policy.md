# Creating a Policy for Amazon SES Sending Authorization<a name="sending-authorization-identity-owner-tasks-policy"></a>

To authorize a delegate sender to send emails using an email address or domain \(an *identity*\) that you own, you create a sending authorization policy, and then attach that policy to the identity\. An identity can have zero, one, or many policies\. However, a single policy can only be associated with a single identity\.

You can create a sending authorization policy in the following ways:
+ **By using the Policy Generator** – You can create a simple policy by using the Policy Generator in the Amazon SES console\. In addition to specifying who can send the emails, you can constrain the email\-sending with conditions based on the time and date range in which emails can be sent, the "From" address, the "From" display name, the address to which bounces and complaints are sent, the recipient addresses, and the source IP\. You might also want to use the Policy Generator to create the structure of a simple policy and then customize it later by editing the policy\.
+ **By creating a Custom Policy** – If you want to include more advanced conditions or use an AWS service as the principal, you can create a custom policy and attach it to the identity by using the Amazon SES console or the Amazon SES API\.

This topic describes both methods\.

**Note**  
Sending authorization policies that you attach to email address identities take precedence over policies that you attach to their corresponding domain identities\. For example, if you create a policy for *example\.com* that disallows a delegate sender, and you create a policy for *sender@example\.com* that allows the delegate sender, then the delegate sender can send email from *sender@example\.com*, but not from any other address on the *example\.com* domain\.  
If you create a policy for *example\.com* that allows a delegate sender, and you create a policy for *sender@example\.com* that disallows the delegate sender, then the delegate sender can send email from any address on the *example\.com* domain, except for *sender@example\.com*\.

## Creating a Policy Using the Policy Generator<a name="sending-authorization-identity-owner-tasks-identity-policy-generator"></a>

You can use the Policy Generator to create a simple authorization policy by using the following procedure\.

**To create a policy by using the Policy Generator**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Identity Management**, choose either **Domains** or **Email Addresses**\.

1. In the list of identities, choose the identity for which you want to create a policy\.

1. In the details pane, expand **Identity Policies**, choose **Create Policy**, and then choose **Policy Generator**\.

1. In the wizard, create a policy statement by choosing values for the following fields\. You can find information about these options in [Sending Authorization Policies](sending-authorization-policies.md)\.
   + **Effect** – If you want to grant access, choose **Allow**; otherwise, choose **Deny**\. 
   + **Principals** – Enter either the 12\-digit AWS account ID or the ARN of an IAM user that you are allowing or denying access, and then choose **Add**\. You can add more principals by repeating this step\. An example of an AWS account ID is 123456789012 and an example of an IAM user ARN is *arn:aws:iam::123456789012:user/John*\.
**Note**  
The policy generator wizard does not currently support AWS service principals\. To add an AWS service principal, you must either [create a custom policy](#sending-authorization-identity-owner-tasks-identity-policy-custom) or use the policy generator to add an AWS account or IAM user principal, and then [edit](sending-authorization-identity-owner-tasks-management.md#sending-authorization-identity-owner-tasks-management-edit) the policy\.
   + **Actions** – Choose the email\-sending access to which this policy applies\. Typically, identity owners choose both options to give the delegate sender the freedom to choose how to implement the email sending\. For more information, see [Statements Specific to the Policy](sending-authorization-policies.md#sending-authorization-policy-statements)\. 

1. \(Optional\) If you want to add restrictions to the policy, choose **Add Conditions**, and then choose the following information:
   + **Key** – This is the characteristic that is the basis for access restriction\. The Policy Generator lets you choose an Amazon SES\-specific key or one of a few commonly used AWS\-wide keys \(current time and source IP\)\. For details, see [Conditions](sending-authorization-policies.md#sending-authorization-policy-conditions)\. If you want to specify the more advanced AWS\-wide keys listed in [Available Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#AvailableKeys), you can edit the policy after you create it\.
   + **Condition** – This is the type of condition that you want to specify\. For example, there are string conditions, numeric conditions, date and time conditions, and so on\. For a list of conditions, see [Condition Types](https://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#AccessPolicyLanguage_ConditionType) in the *IAM User Guide*\.
   + **Value** – This is the value that will be tested against the condition\. For examples, see the policies in [Sending Authorization Policy Examples](sending-authorization-policy-examples.md)\. 

   After you choose the key, condition, and value, choose **Add Condition**\. The condition appears in the **Conditions** list\. You can remove conditions by choosing **Remove** next to a condition in the list\. You can add another condition by choosing **Add Conditions** again\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/sending_authorization_policy_generator.png)

1. When you finish adding conditions, choose **Add Statement**\. The statement appears in the **Statements** list, where you can choose to edit or remove it\. You can add additional statements by repeating steps 5–7\.

1. When you finish adding statements, choose **Next**\.

1. In the **Edit Policy** dialog box, review your policy, edit it if necessary, and then choose **Apply Policy**\.

## Creating a Custom Policy<a name="sending-authorization-identity-owner-tasks-identity-policy-custom"></a>

If you want to create a custom policy and attach it to an identity, you have the following options:
+ **Using the Amazon SES API** – Create a policy in a text editor and then attach the policy to the identity by using the `PutIdentityPolicy` API described in the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\.
+ **Using the Amazon SES console** – Create a policy in a text editor and attach it to an identity by pasting it into the Custom Policy editor in the Amazon SES console\. The following procedure describes this method\.

**To create a custom policy by using the Custom Policy editor**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Identity Management**, choose either **Domains** or **Email Addresses**\.

1. In the list of identities, choose the identity for which you want to create a policy\.

1. In the details pane, expand **Identity Policies**, choose **Create Policy**, and then choose **Custom Policy**\.

1. In the **Edit Policy** pane, paste the text of your policy\.

1. Choose **Apply Policy**\.