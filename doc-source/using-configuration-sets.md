# Using configuration sets in Amazon SES<a name="using-configuration-sets"></a>

Configuration sets are groups of rules that you can apply to your verified identities\. A verified identity is a domain, subdomain, or email address you use to send email through Amazon SES When you apply a configuration set to an email, all of the rules in that configuration set are applied to the email\.

You can use configuration sets to apply the following types of rules to your email sending and can contain one, both, or neither of these types:
+ *Event destinations* – Allow you to publish email sending metrics, including the numbers of sends, deliveries, opens, clicks, bounces, and complaints to other AWS products for each email you send\. For example, you can send your email metrics to an Amazon Kinesis Data Firehose destination, and then analyze it using Amazon Kinesis Data Analytics\. Alternatively, you can send bounce and complaint information to Amazon SNS and receive notifications immediately when those events occur\.
+ *IP pool management* – If you lease dedicated IP addresses to use with Amazon SES, you can create groups of these addresses called *dedicated IP pools* to be used for sending specific types of email\. For example, you can associate these dedicated IP pools with configuration sets and use one for sending marketing communications, and another for sending transactional emails\. Your sender reputation for transactional emails is then isolated from that of your marketing emails\.

To associate a configuration set with a verified identity can be done in the following ways:
+ Include a reference to the configuration set in the headers of the email\. For more information about specifying configuration sets in your emails, see [Specifying a configuration set when you send email](using-configuration-sets-in-email.md)\.
+ Specify an existing configuration set to be used as the identity's *default configuration set*, either at the time of identity creation, or later while editing a verified identity\. See [Understanding default configuration sets](managing-configuration-sets.md#default-config-sets)\.

**Topics**
+ [Creating configuration sets in Amazon SES](creating-configuration-sets.md)
+ [Managing configuration sets in Amazon SES](managing-configuration-sets.md)
+ [Specifying a configuration set when you send email](using-configuration-sets-in-email.md)
+ [Viewing and exporting reputation metrics](configuration-sets-export-metrics.md)