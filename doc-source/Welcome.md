# What is Amazon SES?<a name="Welcome"></a>

[Amazon SES](https://aws.amazon.com/ses) is an email platform that provides an easy, cost\-effective way for you to send and receive email using your own email addresses and domains\.

For example, you can send marketing emails such as special offers, transactional emails such as order confirmations, and other types of correspondence such as newsletters\. When you use Amazon SES to receive mail, you can develop software solutions such as email autoresponders, email unsubscribe systems, and applications that generate customer support tickets from incoming emails\.

For more information about topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\.

## Benefits<a name="why-use-ses"></a>

Building a large\-scale email solution is often a complex and costly challenge for a business\. You must deal with infrastructure challenges such as email server management, network configuration, and IP address reputation\. Additionally, many third\-party email solutions require contract and price negotiations, as well as significant up\-front costs\. Amazon SES eliminates these challenges and enables you to benefit from the years of experience and sophisticated email infrastructure Amazon\.com has built to serve its own large\-scale customer base\.

## Related services<a name="ses-and-aws"></a>

Amazon SES integrates seamlessly with other AWS products\. For example, you can:
+ Add email\-sending capabilities to any application\. If your application runs in [Amazon Elastic Compute Cloud](https://aws.amazon.com/ec2/) \(Amazon EC2\), you can use Amazon SES to [send 62,000 emails every month at no additional charge](https://aws.amazon.com/ses/pricing/)\. You can send email from Amazon EC2 by using an [AWS SDK](https://aws.amazon.com/tools/#sdk), by using the [Amazon SES SMTP interface](send-email-smtp.md), or by making calls directly to the [Amazon SES API](https://docs.aws.amazon.com/ses/latest/APIReference/)\.
+ Use [AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) to create an email\-enabled application such as a program that uses Amazon SES to send a newsletter to customers\.
+ Set up [Amazon Simple Notification Service \(Amazon SNS\)](https://aws.amazon.com/sns/) to notify you of your emails that bounced, produced a complaint, or were successfully delivered to the recipient's mail server\. When you use Amazon SES to receive emails, your email content can be published to Amazon SNS topics\.
+ Use the AWS Management Console to set up Easy DKIM, which is a way to authenticate your emails\. Although you can use Easy DKIM with any DNS provider, it is especially easy to set up when you manage your domain with [Route 53](https://aws.amazon.com/route53/)\.
+ Control user access to your email sending by using [AWS Identity and Access Management \(IAM\)](https://aws.amazon.com/iam/)\.
+ Store emails you receive in [Amazon Simple Storage Service \(Amazon S3\)](https://aws.amazon.com/s3/)\.
+ Take action on your received emails by triggering [AWS Lambda](https://aws.amazon.com/lambda/) functions\.
+ Use [AWS Key Management Service \(AWS KMS\)](https://aws.amazon.com/kms/) to optionally encrypt the mail you receive in your Amazon S3 bucket\.
+ Use [AWS CloudTrail](https://aws.amazon.com/cloudtrail/) to log Amazon SES API calls that you make using the console or the Amazon SES API\.
+ Publish your email sending events to [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) or [Amazon Kinesis Data Firehose](https://aws.amazon.com/firehose/)\. If you publish your email sending events to Kinesis Data Firehose, you can access them in [Amazon Redshift](https://aws.amazon.com/redshift/), [Amazon OpenSearch Service](https://aws.amazon.com/elasticsearch-service/), or [Amazon S3](https://aws.amazon.com/s3/)\.

## Pricing<a name="ses-pricing"></a>

With Amazon SES, you pay based on the volume of emails sent and received\. For more information, see [Amazon SES pricing](http://aws.amazon.com/ses/pricing/)\.