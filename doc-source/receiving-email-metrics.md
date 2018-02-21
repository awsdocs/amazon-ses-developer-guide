# Viewing Metrics for Amazon SES Email Receiving<a name="receiving-email-metrics"></a>

You can use Amazon CloudWatch \(CloudWatch\) to view failure metrics for your receipt rules\. You'll find the metrics under **SES/Rule Metrics**\.

There are two failure metrics:

+ **PublishFailure** – Amazon SES encountered an error when it tried to execute the actions you configured\.

+ **PublishExpired** – Amazon SES encountered an error when it tried to execute the actions you configured, and Amazon SES will no longer retry to deliver the email\. This failure can be permanent or transient\. Amazon SES will no longer retry because the action did not succeed within four hours\.

These errors can occur, for example, if you deleted or revoked permissions to an Amazon S3 bucket, Amazon SNS topic, or Lambda function that an action in one of your receipt rules was configured to use\.

**Important**  
Changes you make to fix your receipt rule set will apply only to emails that Amazon SES receives after the update\. Emails are always evaluated against the receipt rule set that was in place at the time the email was received\.

The following figure shows the metrics in the CloudWatch console\.

![\[Receipt rule errors in CloudWatch.\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/inbound_cloudwatch.png)