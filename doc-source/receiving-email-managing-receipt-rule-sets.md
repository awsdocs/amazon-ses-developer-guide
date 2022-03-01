# Managing receipt rule sets for Amazon SES email receiving<a name="receiving-email-managing-receipt-rule-sets"></a>

After you create a receipt rule set as described in [Creating a receipt rule set](receiving-email-receipt-rule-set.md), you can update it as needed\. Although editing a receipt rule set usually consists of editing individual receipt rules as described in [Managing receipt rules](receiving-email-managing-receipt-rules.md), you can also delete, activate, disable, and copy receipt rule sets\. Additionally, you can reorder the receipt rules in a receipt rule set\. These operations are described in the following sections\. 

**Topics**
+ [Deleting a receipt rule set](#receiving-email-managing-receipt-rule-sets-delete)
+ [Activating and disabling a receipt rule set](#receiving-email-managing-receipt-rule-sets-enable-disable)
+ [Copying a receipt rule set](#receiving-email-managing-receipt-rule-sets-copy)
+ [Reordering receipt rules](#receiving-email-managing-receipt-rule-sets-reorder)

## Deleting a receipt rule set<a name="receiving-email-managing-receipt-rule-sets-delete"></a>

You can use the Amazon SES console or the `DeleteReceiptRuleSet` API to delete a receipt rule set\.

**Note**  
You cannot delete the receipt rule set that is currently active\.

**To delete a receipt rule set \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. In the **Inactive Rule Sets** list, select the receipt rule set that you want to delete\.

1. From the **Actions** menu, choose **Delete**, and then confirm that you want to delete the receipt rule set\.

For information about how to use the `DeleteReceiptRuleSet` API to delete a receipt rule set, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteReceiptRuleSet.html)\.

## Activating and disabling a receipt rule set<a name="receiving-email-managing-receipt-rule-sets-enable-disable"></a>

Each receipt rule set is in one of two states: active or disabled\. Only one of your receipt rule sets can be active at any given time\. Disabled receipt rule sets can be useful in cases where you want to make changes to your active receipt rule set, but you do not want those changes to be active until you are sure your updates are correct\. In that case, you can copy the active receipt rule set and make changes to the copied, disabled receipt rule set\. After you're satisfied with the changes, you can activate the copied receipt rule set\. When you activate a receipt rule set, all other receipt rule sets are disabled automatically\.

**Note**  
To disable email receiving through Amazon SES completely, disable all of your receipt rule sets\.

You can use the Amazon SES console or the `SetActiveReceiptRuleSet` API to control which rule set is active\.

**To activate a disabled receipt rule set \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. In the **Inactive Rule Sets** list, select the receipt rule set that you want to activate\.

1. Choose **Set as Active Rule Set**\.

**To disable the active receipt rule set \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. Under **Active Rule Set**, choose **Disable Active Rule Set**, and then confirm that you want to disable the receipt rule set\.

For information about how to use the `SetActiveReceiptRuleSet` API to activate or disable a rule set, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetActiveReceiptRuleSet.html)\. 

## Copying a receipt rule set<a name="receiving-email-managing-receipt-rule-sets-copy"></a>

You can use the Amazon SES console or the `CloneReceiptRuleSet` API to copy a receipt rule set\. If you use the Amazon SES console, the procedure differs slightly, depending on whether the receipt rule set you want to copy is active or disabled\.

**To copy the active receipt rule set \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. In the content pane, choose **Copy Active Rule Set**\.

1. In the **Copy Rule Set** dialog box, type the name you want to assign to the copied receipt rule set\.

1. Choose **Copy Rule Set**\. The copied receipt rule set will appear in the **Inactive Rule Sets** list\.

**To copy a disabled receipt rule set \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. In the **Inactive Rule Sets** list, select the receipt rule set that you want to copy\.

1. From the **Actions** menu, choose **Copy**\.

1. In the **Copy Rule Set** dialog box, type the name you want to assign to the copied receipt rule set\.

1. Choose **Copy Rule Set**\. The copied receipt rule set will appear in the **Inactive Rule Sets** list\.

For information about how to use the `CloneReceiptRuleSet` API to copy a receipt rule set, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_CloneReceiptRuleSet.html)\.

## Reordering receipt rules<a name="receiving-email-managing-receipt-rule-sets-reorder"></a>

You can use the Amazon SES console or the `ReorderReceiptRuleSet` API to reorder receipt rules in a receipt rule set\. If you use the Amazon SES console, the procedure differs slightly, depending on whether the receipt rule set is active or disabled\.

**To reorder receipt rules in the active receipt rule set \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. In the content pane, choose **View Active Rule Set**\.

1. Choose **Reorder Rules**\.

1. Use the up and down arrows next to the receipt rule names to reorder the receipt rules, and then choose **Save Order**\.

**To reorder receipt rules in a disabled receipt rule set \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. In the **Inactive Rule Sets** list, select the receipt rule set\.

1. Choose **Reorder Rules**\.

1. Use the up and down arrows next to the receipt rule names to reorder the receipt rules, and then choose **Save Order**\.

For information about how to use the `ReorderReceiptRuleSet` API to reorder receipt rules in a receipt rule set, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_ReorderReceiptRuleSet.html)\.
