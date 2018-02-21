# What Is Amazon SES?<a name="Welcome"></a>

Welcome to the Amazon Simple Email Service \(Amazon SES\) Developer Guide\. Amazon SES is an email platform that provides an easy, cost\-effective way for you to send and receive email using your own email addresses and domains\.

For example, you can send marketing emails such as special offers, transactional emails such as order confirmations, and other types of correspondence such as newsletters\. When you use Amazon SES to receive mail, you can develop software solutions such as email autoresponders, email unsubscribe systems, and applications that generate customer support tickets from incoming emails\.

You only pay for what you use, so you can send and receive as much or as little email as you like\. For service highlights, FAQs, and pricing information, go to the [Amazon SES Detail Page](https://aws.amazon.com/ses/)\.

## Why use Amazon SES?<a name="why-use-ses"></a>

Building a large\-scale email solution is often a complex and costly challenge for a business\. You must deal with infrastructure challenges such as email server management, network configuration, and IP address reputation\. Additionally, many third\-party email solutions require contract and price negotiations, as well as significant up\-front costs\. Amazon SES eliminates these challenges and enables you to benefit from the years of experience and sophisticated email infrastructure Amazon\.com has built to serve its own large\-scale customer base\. 

## Amazon SES and other AWS services<a name="ses-and-aws"></a>

Amazon SES integrates seamlessly with other AWS products\. For example, you can:

+ Add email capabilities to any application that runs on an [Amazon Elastic Compute Cloud \(Amazon EC2\)](https://aws.amazon.com/ec2/) instance by using the [AWS SDKs](https://aws.amazon.com/tools/) or the Amazon SES API\. If you want to send email through Amazon SES from an Amazon EC2 instance, you can get started with Amazon SES for [free](https://aws.amazon.com/ses/pricing/)\.

+ Use [AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) to create an email\-enabled application such as a program that uses Amazon SES to send a newsletter to customers\.

+ Set up [Amazon Simple Notification Service \(Amazon SNS\)](https://aws.amazon.com/sns/) to notify you of your emails that bounced, produced a complaint, or were successfully delivered to the recipient's mail server\. When you use Amazon SES to receive emails, your email content can be published to Amazon SNS topics\.

+ Use the AWS Management Console to set up Easy DKIM, which is a way to authenticate your emails\. Although you can use Easy DKIM with any DNS provider, it is especially easy to set up when you manage your domain with [Route 53](https://aws.amazon.com/route53/)\.

+ Control user access to your email sending by using [AWS Identity and Access Management \(IAM\)](https://aws.amazon.com/iam/)\.

+ Store emails you receive in [Amazon Simple Storage Service \(Amazon S3\)](https://aws.amazon.com/s3/)\.

+ Take action on your received emails by triggering [AWS Lambda](https://aws.amazon.com/lambda/) functions\.

+ Use [AWS Key Management Service \(AWS KMS\)](https://aws.amazon.com/kms/) to optionally encrypt the mail you receive in your Amazon S3 bucket\.

+ Use [AWS CloudTrail](https://aws.amazon.com/cloudtrail/) to log Amazon SES API calls that you make using the console or the Amazon SES API\.

+ Publish your email sending events to [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) or [Amazon Kinesis Firehose](https://aws.amazon.com/firehose/)\. If you publish your email sending events to Kinesis Firehose, you can access them in [Amazon Redshift](https://aws.amazon.com/redshift/), [Amazon Elasticsearch Service](https://aws.amazon.com/elasticsearch-service/), or [Amazon S3](https://aws.amazon.com/s3/)\.

## In this guide<a name="in-this-guide"></a>

This guide contains the following sections:


****  

| Section | Description | 
| --- | --- | 
| [Sending Email](sending-email.md) | Describes how you can send email using Amazon SES\. | 
| [Receiving Email](receiving-email.md) | Describes how you can receive email using Amazon SES\. | 
| [Controlling Access](control-user-access.md) | Shows you how to use Amazon SES with AWS Identity and Access Management \(IAM\) to specify which Amazon SES API actions a user can perform on which Amazon SES resources\. | 
| [Logging API Calls](logging-using-cloudtrail.md) | Provides a list of Amazon SES APIs that can be logged using AWS CloudTrail\. | 
| [Using Credentials](using-credentials.md) | Explains the types of credentials that you might use with Amazon SES, and when you might use them\. | 
| [Using the API](using-the-api.md) | Describes how to use the Amazon SES Query API\. | 
| [Regions](regions.md) | Lists the Amazon SES SMTP and API endpoints for the AWS regions in which Amazon SES is available, and contains information you need to know when you use Amazon SES endpoints in multiple regions\. | 
| [Limits](limits.md) | Provides a list of limits within Amazon SES\. | 
| [Resources](resources.md) | Lists resources that you may find useful as you work with Amazon SES | 
| [Appendix](appendix.md) | Provides supplementary information about header fields, unsupported attachment types, and scripts\. | 


****  

|  | 
| --- |
| For technical discussions about various Amazon SES topics, visit the [Amazon SES Blog](https://aws.amazon.com//blogs/ses/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 