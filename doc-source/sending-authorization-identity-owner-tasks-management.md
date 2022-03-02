# Managing your policies for Amazon SES sending authorization<a name="sending-authorization-identity-owner-tasks-management"></a>

In addition to creating and attaching policies to identities as explained in [Creating a policy](sending-authorization-identity-owner-tasks-policy.md), you can edit, remove, list, and retrieve an identity's policies, as described in the following sections\.

**Note**  
To revoke permissions, you can either edit a policy or remove it\.

## Editing a policy<a name="sending-authorization-identity-owner-tasks-management-edit"></a>

We recommend using the Amazon SES console to edit a policy\. If you want to use the Amazon SES API instead, you can use the [GetIdentityPolicies](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityPolicies.html) operation to retrieve the policy, edit the policy using a text editor, and then use the [PutIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html) operation to overwrite the older policy\.

The following steps show you how to edit a policy by using the Amazon SES console\.

**To edit a policy by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the list of identities, choose the identity that is associated with the policy that you want to edit\.

1. In the identity's detail pane, choose the **Authorization** tab\.

1. Select the checkbox next to the policy you want to edit, and choose **Edit**\.

1. In the **Policy document** pane, edit the policy, and then choose **Save changes**\.

## Removing a policy<a name="sending-authorization-identity-owner-tasks-management-remove"></a>

To revoke permissions at any time, you can simply remove the policy\. You can remove a policy by using the [DeleteIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteIdentityPolicy.html) API operation, or you can use the Amazon SES console, as described in the following procedure\.

**Important**  
After you remove a policy, there is no way to get it back\. We recommend that you back up the policy by copying and pasting it into a text file before you remove the policy\.

**To remove a policy by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the list of identities, choose the identity that is associated with the policy that you want to remove\.

1. In the identity's detail pane, choose the **Authorization** tab\.

1. Select the checkbox next to the policy you want to remove, and choose **Delete**\.

1. In the **Delete Policy?** confirmation popup, choose **Delete**\.

## Listing and retrieving policies<a name="sending-authorization-identity-owner-tasks-management-list"></a>

You can list the policies that are attached to an identity by using the [ListIdentityPolicies](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListIdentityPolicies.html) API operation\. You can also retrieve the policies themselves by using the [GetIdentityPolicies](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityPolicies.html) API operation\.

You can also use the Amazon SES console to perform both of these tasks, as described in the following procedure\.

**To list and view the policies attached to an identity by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the list of identities, choose the identity that is associated with the policy that you want to view\.

1. In the identity's detail pane, choose the **Authorization** tab\.

1. Select the checkbox next to the policy you want to view, and choose **View policy**\.

1. In the **View Policy** popup, you can view your policy, and if you need a copy of it, choose the **Copy** button and it will be copied to your clipboard\.