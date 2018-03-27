# Step 3: Set up a Receipt Rule<a name="receiving-email-getting-started-receipt-rule"></a>

To use Amazon SES as your email receiver, you must have at least one active *receipt rule set*\. A receipt rule set is an ordered collection of *receipt rules* that specify what Amazon SES should do with mail it receives across all of your verified domains\. Because you are setting up email receiving with Amazon SES for the first time, Amazon SES automatically creates a default receipt rule set for you, so you can jump into creating a receipt rule\. The receipt rule you create will belong to the default receipt rule set\. 

**To create a receipt rule**

1. In the left navigation pane, under **Email Receiving**, choose **Rule Sets**\. Then, in the content pane, choose **Create a Receipt Rule**\.   
![\[Create a receipt rule\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_rule_1.png)

1. On the **Recipients** page, choose **Next Step**\. Because you aren't adding any recipients, this receipt rule will handle mail for all recipients in all of your verified domains\.  
![\[Create a receipt rule\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_rule_2.png)

1. From the **Add action** menu, choose **S3**\.  
![\[Create a receipt rule\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_rule_3.png)

1. For **S3 bucket**, choose **Create S3 bucket**\.  
![\[Create a receipt rule\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_rule_4.png)

1. For **Bucket Name**, enter a new bucket name\. Bucket names must comply with the following requirements\.
   + Can contain lowercase letters, numbers, periods \(\.\), and hyphens \(\-\)\.
   + Must be unique across all of AWS\.
   + Must start with a number or letter\.
   + Must be between 3 and 63 characters long\.
   + Must not contain underscores \(\_\), end with a hyphen, or be formatted as an IP address \(e\.g\., 192\.168\.5\.4\)\.
   + Cannot contain two, adjacent periods or dashes next to periods\.

   Next, choose **Create Bucket**\.  
![\[Create a receipt rule\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_rule_5.png)
**Note**  
Because you are creating the Amazon S3 bucket using the Amazon SES console, Amazon SES automatically sets up the policy required to give Amazon SES permission to write to the bucket\. However, if you choose an Amazon S3 bucket that already exists, you must explicitly give Amazon SES permission to write to the bucket by attaching a [policy](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/receiving-email-permissions.html) to the bucket using the Amazon S3 console or API\.

1. Leave all other options at their default settings for the simplicity of this tutorial, and choose **Next Step**\.  
![\[Create a receipt rule\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_rule_6.png)

1. On the **Rule Details** page, for the rule name, type **my\-rule**, leave all other options at their default settings, and then choose **Next Step**\.  
![\[Create a receipt rule\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_rule_7.png)

1. On the **Review** page, choose **Create Rule**\.  
![\[Create a receipt rule\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_rule_8.png)

Next step: [Step 4: Send an Email](receiving-email-getting-started-send.md)