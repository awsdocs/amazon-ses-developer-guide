# Using Amazon SES Configuration Sets<a name="using-configuration-sets"></a>

Configuration sets are groups of rules that you can apply to the emails you send using Amazon SES\. You apply a configuration set to an email by including a reference to the configuration set in the headers of the email\. When you apply a configuration set to an email, all of the rules in that configuration set are applied to the email\. For more information about specifying configuration sets in your emails, see [Specifying a Configuration Set When You Send Email](using-configuration-sets-in-email.md)\.

You can use configuration sets to apply the following types of rules to your emails:

+ **Event publishing** – Amazon SES can track the number of send, delivery, open, click, bounce, and complaint events for each email you send\. You can use event publishing to send information about these events to other AWS services\. For example, you can send your email metrics to an Amazon Kinesis Data Firehose destination, and then analyze it using Amazon Kinesis Data Analytics\. Alternatively, you can send bounce and complaint information to Amazon SNS and receive notifications immediately when those events occur\.

+ **IP pool management** – If you lease dedicated IP addresses to use with Amazon SES, you can create groups of these addresses, called *dedicated IP pools*\. You can then associate these dedicated IP pools with configuration sets\. A common use case is to create one pool of dedicated IP addresses for sending marketing communications, and another for sending transactional emails\. Your sender reputation for transactional emails is then isolated from that of your marketing emails\.

Configuration sets can contain one, both, or neither of these types of rules\.

To learn more about managing configuration sets and their related components, see the following topics:

+ [Managing Amazon SES Configuration Sets](managing-configuration-sets.md)

+ [Managing Amazon SES Event Destinations](event-publishing-managing-event-destinations.md)

+ [Managing IP Pools](managing-ip-pools.md)