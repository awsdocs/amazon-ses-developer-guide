# Managing Receipt Rules for Amazon SES Email Receiving<a name="receiving-email-managing-receipt-rules"></a>

In addition to creating receipt rules as described in [Creating Receipt Rules](receiving-email-receipt-rules.md), you can edit, delete, enable, disable, copy, and set the position of a receipt rule in its receipt rule set, as described in the following sections\.

**Note**  
The instructions in this section assume that the receipt rule is in the active receipt rule set\. To edit the receipt rules of a disabled receipt rule set, choose a receipt rule set from the **Inactive Rule Sets** list\. From there, the instructions for editing receipt rules are the same as for the active receipt rule set\.

**Topics**
+ [Editing a Receipt Rule](#receiving-email-managing-receipt-rules-edit)
+ [Deleting a Receipt Rule](#receiving-email-managing-receipt-rules-delete)
+ [Enabling and Disabling a Receipt Rule](#receiving-email-managing-receipt-rules-enable-disable)
+ [Copying a Receipt Rule](#receiving-email-managing-receipt-rules-copy)
+ [Setting the Position of a Receipt Rule](#receiving-email-managing-receipt-rules-set-position)

## Editing a Receipt Rule<a name="receiving-email-managing-receipt-rules-edit"></a>

You can use the Amazon SES console or the Amazon SES API to edit a receipt rule\. It is easier to use the Amazon SES console\. 

**To edit a receipt rule \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. In the content pane, choose **View Active Rule Set** or choose a receipt rule set from the **Inactive Rule Sets** list\.

1. In the details pane, choose the receipt rule you want to edit\.

1. In the **Edit Rule** pane, edit the policy, and then choose **Save Rule**\.

If you want to use the Amazon SES API instead, use the `DescribeReceiptRule` API to retrieve the rule, use a text editor to edit the rule, and then use the `UpdateReceiptRule` API to overwrite the previous version of the rule\. For more information, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/API_UpdateReceiptRule.html)\.

## Deleting a Receipt Rule<a name="receiving-email-managing-receipt-rules-delete"></a>

You can use the Amazon SES console or the `DeleteReceiptRule` API to delete a receipt rule\.

**To delete a receipt rule \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. In the content pane, choose **View Active Rule Set** or choose a receipt rule set from the **Inactive Rule Sets** list\.

1. In the details pane, select the receipt rule\.

1. From the **Actions** menu, choose **Delete**, and then confirm that you want to delete the receipt rule\.

For information about how to use the `DeleteReceiptRule` API to delete a rule, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteReceiptRule.html)\.

## Enabling and Disabling a Receipt Rule<a name="receiving-email-managing-receipt-rules-enable-disable"></a>

You can use the Amazon SES console or the Amazon SES API to enable or disable a receipt rule\. It is easier to use the Amazon SES console\.

**To enable or disable a receipt rule \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. In the content pane, choose **View Active Rule Set** or choose a receipt rule set from the **Inactive Rule Sets** list\.

1. In the details pane, choose the receipt rule you want to edit\.

1. In the **Edit Rule** pane, select or clear **Enabled**, and then choose **Save Rule**\.

If you want to use the Amazon SES API instead, you can use the `DescribeReceiptRule` API to retrieve the receipt rule, use a text editor to edit the receipt rule's `Enabled` field, and then use the `UpdateReceiptRule` API to overwrite the previous version of the receipt rule\. For more information, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/API_UpdateReceiptRule.html)\.

## Copying a Receipt Rule<a name="receiving-email-managing-receipt-rules-copy"></a>

You can use the Amazon SES console or the Amazon SES API to copy a receipt rule\. It is easier to use the Amazon SES console\.

**To copy a receipt rule \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. In the content pane, choose **View Active Rule Set** or choose a receipt rule set from the **Inactive Rule Sets** list\.

1. In the details pane, select the receipt rule\.

1. From the **Actions** menu, choose **Copy Rule**\.

1. In the **Copy Rule** dialog box, type a new receipt rule name and select the destination receipt rule set\. The new receipt rule will be inserted at the beginning of the receipt rule set, and it will initially be disabled\. 

If you want to use the Amazon SES API instead, you can use the `DescribeReceiptRule` API to retrieve the receipt rule, use a text editor to edit the receipt rule's name and receipt rule set \(if desired\), and then pass that receipt rule to the `CreateReceiptRule` API\. For more information, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptRule.html)\. 

## Setting the Position of a Receipt Rule<a name="receiving-email-managing-receipt-rules-set-position"></a>

You can use the Amazon SES console or the `SetReceiptRulePosition` API to change the position of a receipt rule in the receipt rule set\.

**To set the position of a receipt rule \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. In the content pane, choose **View Active Rule Set** or choose a receipt rule set from the **Inactive Rule Sets** list\.

1. In the content pane, choose **Reorder Rules**\.

1. Use the up and down arrows next to the receipt rule names to reorder the receipt rules, and then choose **Save Order**\.

For information about how to use the `SetReceiptRulePosition` API to change the position of a receipt rule in the receipt rule set, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/API_SetReceiptRulePosition.html)\.