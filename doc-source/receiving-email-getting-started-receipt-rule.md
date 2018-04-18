# Step 3: Set up a Receipt Rule<a name="receiving-email-getting-started-receipt-rule"></a>

To use Amazon SES as your email receiver, you must have an active *receipt rule set*\. A receipt rule set is a collection of *receipt rules* that specify what Amazon SES should do with mail it receives for your verified domains\. Because you're setting up email receiving with Amazon SES for the first time, Amazon SES automatically creates a default receipt rule set for you\. The receipt rule you create in this section belongs to the default receipt rule set\.

**Note**  
The procedures in this section assume you've never created a receipt rule set\. If your account already contains a receipt rule set, you'll need to make the receipt rule you create in this section active before Amazon SES applies it to the incoming email for your domain\. For more information about enabling and disabling receipt rule sets, see [Activating and Disabling a Receipt Rule Set](receiving-email-managing-receipt-rule-sets.md#receiving-email-managing-receipt-rule-sets-enable-disable)\.

**To create a receipt rule**

1. In the navigation pane, under **Email Receiving**, choose **Rule Sets**\.

1. Choose **Create a Receipt Rule**\.

1. On the **Recipients** page, choose **Next Step**\.
**Note**  
Because you aren't adding any recipients, Amazon SES applies this rule to all recipients across all of your verified domains\.

1. For **Add action**, choose **S3**\.  
![\[Create a receipt rule\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_rule_3.png)

1. For **S3 bucket**, choose **Create S3 bucket**\.  
![\[Create a receipt rule\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_rule_4.png)

1. For **Bucket Name**, type a name for the Amazon S3 bucket\. The bucket name you enter must meet the following requirements:
   + It can only contain lowercase letters, numbers, periods \(\.\), and hyphens \(\-\)\.
   + It must be unique across all of AWS\.
   + It must start and end with a number or a lowercase letter\.
   + It must contain at least 3 characters, and no more than 63 characters\.
   + It can't be formatted as an IP address \(for example, 192\.168\.5\.4\)\.
   + It can't contain two adjacent periods \(\.\.\) or a dash adjacent to a period \(\-\. or \.\-\)\.

   When you finish, choose **Create Bucket**\.
**Note**  
Because you're using the Amazon SES console to create an Amazon S3 bucket, Amazon SES automatically creates and applies a policy that gives it permission to write to the bucket\. However, if you choose an existing Amazon S3 bucket, you must give Amazon SES permission to write to the bucket by [attaching a policy to the bucket](receiving-email-permissions.md) using the Amazon S3 console or API\.

1. Choose **Next Step**\.

1. On the **Rule Details** page, for **Rule name**, type **my\-rule**\. Select the check box next to **Enabled**, and then choose **Next Step**\.  
![\[Create a receipt rule\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_rule_7.png)

1. On the **Review** page, choose **Create Rule**\.

Next step: [Step 4: Send a Test Email](receiving-email-getting-started-send.md)