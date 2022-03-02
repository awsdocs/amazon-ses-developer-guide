# Creating receipt rules console walkthrough<a name="receiving-email-receipt-rules-console-walkthrough"></a>

This section will walk you through creating and defining receipt rules using the Amazon SES console\. The key points to understanding how receipt rules work are:
+ *Rule sets* contain an ordered set of receipt rules; *Receipt rules* contain an ordered set of actions\.
+ Receipt rules tell Amazon SES how to handle incoming mail by executing an ordered list of actions you specify\.
+ This ordered list of actions can optionally be made dependant on first matching a recipient condition; if not specified, the actions will be applied to all identities that belong to your verified domains\.
+ Receipt rules are created and defined in a container called a rule set \- while you can create multiple rule sets, only one can be active at a time\.
+ Receipt rules within the active rule set are executed in the order that you specify\.
+ Before you create your receipt rules, you must first create a *rule set* to contain them\.

Optionally, you can use the `CreateReceiptRuleSet` API to create an empty receipt rule set, as described in the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptRuleSet.html)\. Then, you can use the Amazon SES console or the `CreateReceiptRule` API to add receipt rules to it\.

Before proceeding with the walkthrough, please ensure you have met all of the necessary prerequisites that are required in order to use recipient\-based email receiving\. Also

## Prerequisites<a name="receipt-rules-prerequisites"></a>

The following prerequisites must be met before proceeding with setting up recipient based email control using receipt rules: 

1. Ensure your endpoint is in an AWS Region where Amazon SES supports email receiving\. See [supported email receiving endpoints](regions.md#region-receive-email)\.

1. You first need to [create and verify a domain identity](verify-addresses-and-domains.md) in Amazon SES\.

1. Next, you need to specify which mail servers can accept mail for your domain by [publishing an MX record](receiving-email-mx-record.md) to your domain's DNS settings\. \(The MX record should refer to the Amazon SES endpoint that receives mail for the AWS Region where you use Amazon SES\.\)

1. Lastly, you need to [give Amazon SES permission](receiving-email-permissions.md) to access other AWS resources in order to execute receipt rule actions\.

## Creating rule sets and receipt rules<a name="receipt-rules-create-rule-settings"></a>

This walkthrough begins by first creating a rule set to contain your rules and progresses into the **Create rule** wizard to create, define, and order your receipt rules\. The wizard contains four screens to define rule settings, add recipient conditions, add actions, and to review all your settings\.

**To create a rule set and receipt rules using the console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Email Receiving**\.

1. Under **Receipt rule sets**, choose **Create rule set**\.

1. Enter an unique name for your rule set and choose **Create rule set**\.

1. Choose **Create rule** and this will open the **Create rule** wizard\.

1. On the **Define rule settings** page, under **Receipt rule details**, enter a **Rule name**\.

1. For **Status**, only clear the **Enabled** checkbox if you don't want Amazon SES to run this rule after creation; otherwise, leave this option selected\.

1. \(Optional\) Under **Security and protection options**, for **Transport Layer Security \(TLS\)**, select **Required** if you want Amazon SES to reject incoming messages that aren't sent over a secure connection\.

1. \(Optional\) For **Spam and virus scanning**, select **Enabled** if you want Amazon SES to scan incoming messages for spam and viruses\.

1. To proceed to the next step, choose **Next**\.

1. \(Optional\) On the **Add recipient conditions** page, use the following procedure to specify one or more recipient conditions\. You can have a maximum of 100 recipient conditions per receipt rule\.

   1. Under **Recipient conditions**, choose **Add new recipient condition** to specify the receiving email address or domain to which you want to apply the receipt rule\. The following table uses the address *user@example\.com* to show how to specify recipient conditions\.   
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/dg/receiving-email-receipt-rules-console-walkthrough.html)
**Important**  
If multiple Amazon SES accounts receive email on a common domain \(for example, if multiple teams in the same company each have separate Amazon SES accounts\), Amazon SES processes all matching receipt rules simultaneously for each of those accounts\. This behavior may result in a situation where one account generates a bounce, while another account accepts the email\.  
We recommend that you coordinate with other teams in your organization that use Amazon SES to ensure that each account uses unique receipt rules, and that those rules do not overlap\. In these situations, it is best to configure your receipt rules to use only email addresses or subdomains that are unique to your group or team\.

   1. Repeat this step for each recipient condition you want to add\. When you finish adding recipient conditions, choose **Next**\.

1. On the **Add actions** page, use the following procedure to add one or more actions to the receipt rule\.

   1. Open the **Add new action** menu, and then choose one of the following types of actions:
      + **[Add header](receiving-email-action-add-header.md)** \- This action adds a custom header to the received email\.
      + **[Return bounce response](receiving-email-action-bounce.md)** \- This action rejects the received email by returning a bounce response to the sender\.
      + **[Invoke Lambda function](receiving-email-action-lambda.md)** \- This action calls your code via an AWS Lambda function\.
      + **[Deliver to S3 bucket](receiving-email-action-s3.md)** \- This action stores the received email in an Amazon Simple Storage Service \(S3\) bucket\.
      + **[Publish to Amazon SNS topic](receiving-email-action-sns.md)** \- This action publishes the complete email to an Amazon Simple Notification Service \(SNS\) topic\.
      + **[Stop rule set](receiving-email-action-stop.md)** \- This action terminates the evaluation of the receipt rule set\.
      + **[Integrate with Amazon WorkMail](receiving-email-action-workmail.md)** \- This action integrates with Amazon WorkMail\.

      For more information about each of these actions, see [Action options](receiving-email-action.md)\.

   1. Repeat this step for each action that you want to define\. If you have multiple actions defined, you can reorder them by using the up/down arrows within the action containers\. Choose **Next** to proceed to the **Review** page\.

1. On the **Review** page, review the settings and actions of the rule\. If you need to make changes, choose the **Edit** option, or use the navigation section on the left side of the page to go directly to the step that contains the content you want to edit\. You can optionally make changes to the order of the actions listed in the **Actions** table of the **Review** page by using the up/down arrows in the **Reorder** column\.

1. When you’re ready to proceed, choose **Create rule**\.

### Rule modifications after creation<a name="receipt-rules-post-modifications"></a>

After you've created a rule set, you can edit both the rule set and the receipt rules it contains\. Not only can they be edited, but there's also the option to duplicate either the rule set or its rules so that new ones can be created quickly\. The following list shows the available modifications for the rule set and the receipt rules:
+ **Rule set** is listed with its name, status and creation date\. Modification options for the rule set are:
  + **Set as active/inactive** toggle button will toggle between setting the status\.
  + **Duplicate** button will copy the rule set\. You will be prompted to supply a unique name\.
  + **Delete** button will delete the rule set\. You will be prompted to confirm this irreversible action\.
+ **Receipt rules** are listed with their name, status, security, and order\. Modification options for the receipt rules are:
  + **Up/down arrows** to reorder rule execution within the rule set\.
  + **Duplicate** button will create a copy of the selected rule\. You will be prompted to supply a unique name\.
  + **Edit** button will open the selected rule so that any of its parameters such as rule settings, recipient conditions, and actions can be edited\.
  + **Delete** button will delete the selected rule\. You will be prompted to confirm this irreversible action\.
  + **Create rule** button will allow you to create and add a new rule to the current rule set\.