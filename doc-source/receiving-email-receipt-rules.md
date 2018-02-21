# Creating Receipt Rules for Amazon SES Email Receiving<a name="receiving-email-receipt-rules"></a>

Receipt rules let you specify what Amazon SES does with email it receives for the email addresses or domains you own\. A receipt rule contains a condition and an ordered list of actions\. If the recipient of an incoming email matches a recipient specified in the conditions for the receipt rule, then Amazon SES performs the actions specified in that receipt rule\. For more information about the role of receipt rules in the email\-receiving process, see [Email\-Receiving Concepts](receiving-email-concepts.md)\.

**Important**  
To set up receipt rules, first verify a domain and publish an MX record on that domain\. For more information about verifying domains, see [Verifying Domains in Amazon SES](verify-domains.md)\. For more information about publishing MX records, see [[ERROR] BAD/MISSING LINK TEXT](receiving-email-mx-record.md)\.

You can use the Amazon SES console or the `CreateReceiptRule` API operation to create receipt rules\. This section provides procedures for creating a new receipt rule using the console\. These procedures assume that your Amazon SES account does not contain any existing receipt rules\.

## Setting Up a Receipt Rule<a name="receiving-email-receipt-rules-set-up"></a>

You can use the Amazon SES console or the `CreateReceiptRule` API to create rules\.

**To create a receipt rule using the console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. Choose **Create a Receipt Rule**\.

1. Use the following procedure to add one or more recipients\. Collectively, these recipients are the *condition*\. You can have a maximum of 100 recipients per receipt rule\.

   1. Under **Recipients**, specify the incoming email address or domain for which you want to set up a receipt rule\. The following table uses the address *user@example\.com* to show how to specify recipients\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/receiving-email-receipt-rules.html)
**Important**  
If multiple Amazon SES accounts receive email on a common domain \(for example, if multiple teams in the same company each have separate Amazon SES accounts\), Amazon SES processes all matching receipt rules simultaneously for each of those accounts\. This behavior may result in a situation where one account generates a bounce, while another account accepts the email\.  
We recommend that you coordinate with other teams in your organization that use Amazon SES to ensure that each account uses unique receipt rules, and that those rules do not overlap\. In these situations, it is best to configure your receipt rules to use only email addresses or subdomains that are unique to your group or team\.

   1. Choose **Add Recipient**\.

   1. Repeat steps a and b for each recipient you want to add\. When you finish adding recipients, choose **Next Step**\.

1. Use the following procedure to add one or more actions to the receipt rule\.

   1. Choose an action from the menu\.

   1. Choose the action settings\. For information about the options for each action, see [Action Options](receiving-email-action.md)\.

   1. Add additional actions as needed, and then choose **Next Step**\.

1. For **Rule Details**, use the following procedure to choose settings\.

   1. For **Rule Name**, type a name for the receipt rule\. The name must contain less than 64 alphanumeric, hyphen \(\-\), underscore \(\_\), and period \(\.\) characters\. The name must start and end with a letter or number\.

   1. If you want to enable the receipt rule, leave the **Enabled** option selected\.

   1. If you want Amazon SES to reject any incoming emails that are not sent over a connection that is encrypted with Transport Layer Security \(TLS\), select **TLS**\. 

   1. If you want Amazon SES to scan incoming emails for spam and viruses, select **Enable Spam and Virus Scanning**\.

1. For **Rule Set**, choose an existing receipt rule set or click **Create New Rule Set**\.

1. For **Rule Position**, choose where to place the receipt rule in the ordered list of receipt rules\. The receipt rules are evaluated sequentially\.

1. Choose **Next Step**, and then choose **Create Rule**\.

For information about how to use the `CreateReceiptRule` API to create rules, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptRule.html)\. 