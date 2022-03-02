# Data protection in Amazon Simple Email Service<a name="data-protection"></a>

The AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) applies to data protection in Amazon Simple Email Service\. As described in this model, AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. This content includes the security configuration and management tasks for the AWS services that you use\. For more information about data privacy, see the [Data Privacy FAQ](http://aws.amazon.com/compliance/data-privacy-faq)\. For information about data protection in Europe, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\)\. That way each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.
+ If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

We strongly recommend that you never put confidential or sensitive information, such as your customers' email addresses, into tags or free\-form fields such as a **Name** field\. This includes when you work with Amazon Simple Email Service or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into tags or free\-form fields used for names may be used for billing or diagnostic logs\. If you provide a URL to an external server, we strongly recommend that you do not include credentials information in the URL to validate your request to that server\.

**Topics**
+ [Encryption at rest](#encryption-rest)
+ [Encryption in transit](#encryption-transit)
+ [Deleting personal data from Amazon SES](deleting-personal-data.md)

## Encryption at rest<a name="encryption-rest"></a>

Amazon SES integrates with AWS Key Management Service \(AWS KMS\) to encrypt the mail that it writes to your S3 bucket\. Amazon SES uses client\-side encryption to encrypt your mail before it sends it to Amazon S3\. This means that it is necessary for you to decrypt the content on your side after you retrieve the mail from Amazon S3\. The AWS SDK for Java and AWS SDK for Ruby provide a client that is able to handle the decryption for you\.

## Encryption in transit<a name="encryption-transit"></a>

By default, Amazon SES uses opportunistic TLS\. This means that Amazon SES always attempts to make a secure connection to the receiving mail server\. If it can't establish a secure connection, it sends the message unencrypted\. You can change this behavior so that Amazon SES sends the message to the receiving email server only if it can establish a secure connection\. For more information, see [Amazon SES and security protocols](security-protocols.md)\.