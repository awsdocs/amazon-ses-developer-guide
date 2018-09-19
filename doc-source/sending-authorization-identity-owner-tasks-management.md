# Managing Your Policies for Amazon SES Sending Authorization<a name="sending-authorization-identity-owner-tasks-management"></a>

In addition to creating and attaching policies to identities as explained in [Creating a Policy](sending-authorization-identity-owner-tasks-policy.md), you can edit, remove, list, and retrieve an identity's policies, as described in the following sections\.

**Note**  
To revoke permissions, you can either edit a policy or remove it\.

## Editing a Policy<a name="sending-authorization-identity-owner-tasks-management-edit"></a>

The easiest way to edit a policy is to use the Amazon SES console\. If you want to use the Amazon SES API instead, you can use the [GetIdentityPolicies](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityPolicies.html) operation to retrieve the policy, edit the policy using a text editor, and then use the [PutIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html) operation to overwrite the older policy\.

The following procedure shows you how to edit a policy by using the Amazon SES console\.

**To edit a policy by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Identity Management**, choose either **Domains** or **Email Addresses**\.

1. In the list of identities, choose the identity that is associated with the policy that you want to edit\.

1. In the details pane, expand **Identity Policies**\. 

1. Next to the policy that you want to edit, choose **Edit Policy**\.

1. In the **Edit Policy** pane, edit the policy, and then choose **Apply Policy**\.

1. In the **Overwrite Existing Policy** dialog box, choose **Overwrite**\.

## Removing a Policy<a name="sending-authorization-identity-owner-tasks-management-remove"></a>

To revoke permissions at any time, you can simply remove the policy\. You can remove a policy by using the [DeleteIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteIdentityPolicy.html) API operation, or you can use the Amazon SES console, as described in the following procedure\.

**Important**  
After you remove a policy, there is no way to get it back\. We recommend that you back up the policy by copying and pasting it into a text file before you remove the policy\.

**To remove a policy by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Identity Management**, choose either **Domains** or **Email Addresses**\.

1. In the list of identities, choose the identity that is associated with the policy that you want to remove\.

1. In the details pane, expand **Identity Policies**\. Next to the policy that you want to remove, choose **Remove Policy**\.

1. In the **Remove Policy** dialog box, choose **Yes, Remove Policy**\.

## Listing and Retrieving Policies<a name="sending-authorization-identity-owner-tasks-management-list"></a>

You can list the policies that are attached to an identity by using the [ListIdentityPolicies](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListIdentityPolicies.html) API operation\. You can also retrieve the policies themselves by using the [GetIdentityPolicies](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityPolicies.html) API operation\.

You can also use the Amazon SES console to perform both of these tasks, as described in the following procedure\.

**To list and show the policies attached to an identity by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Identity Management**, choose either **Domains** or **Email Addresses**\.

1. In the list of identities, choose the identity for which you want to see policies\.

1. In the details pane, expand **Identity Policies**\.

1. Next to the policy that you want to view, choose **Show Policy**\.