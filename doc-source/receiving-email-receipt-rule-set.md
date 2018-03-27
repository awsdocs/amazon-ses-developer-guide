# Creating a Receipt Rule Set for Amazon SES Email Receiving<a name="receiving-email-receipt-rule-set"></a>

A receipt rule set is an ordered collection of receipt rules that specify what Amazon SES should do with mail it receives across all of your domains\. To use Amazon SES as your email receiver, you must create at least one receipt rule set for your account\. You can set up multiple receipt rule sets, but only one receipt rule set in each AWS region can be active at any given time\. For more information about the role of receipt rule sets in the email\-receiving process, see [Email\-Receiving Concepts](receiving-email-concepts.md)\.

**Note**  
If you do not want to use Amazon SES as your email receiver, simply disable all of your receipt rule sets\. For information about how to disable receipt rule sets, see [Managing Receipt Rule Sets](receiving-email-managing-receipt-rule-sets.md)\.

You can use the Amazon SES console or API to create a receipt rule set\.
+ **Using the Amazon SES console**
  + Receipt rules exist in receipt rule sets only, so to create a receipt rule set, you can start by creating a receipt rule\. For more information, see [Creating Receipt Rules](receiving-email-receipt-rules.md)\. When you reach the end of this procedure, you can create a new receipt rule set\.
  + Copy an existing receipt rule set as explained in [Managing Receipt Rule Sets](receiving-email-managing-receipt-rule-sets.md)\.
  + In the left navigation pane, under **Email Receiving**, choose **Rule Sets**, and then choose **Create a New Rule Set**\.
+ **Using the Amazon SES API—**Use the `CreateReceiptRuleSet` API to create an empty receipt rule set, as described in the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptRuleSet.html)\. Then, you can use the Amazon SES console or the `CreateReceiptRule` API to add receipt rules to it\.