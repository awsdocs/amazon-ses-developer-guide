# Amazon SES Developer Guide document history<a name="doc-history"></a>

The following table lists the major changes to the *Amazon Simple Email Service \(Amazon SES\) Developer Guide*\.


****  

| Change | Description | Date Changed | 
| --- | --- | --- | 
| New feature | You can now receive [notifications](monitor-using-event-publishing.md) when an event occurs that causes the delivery of an email to be temporarily delayed\. | June 19, 2020 | 
| New feature | Amazon SES is now available in [AWS GovCloud \(US\)](regions.md)\. | April 30, 2020 | 
| New feature | You can now [create Amazon SES endpoints in Amazon Virtual Private Cloud](send-email-set-up-vpc-endpoints.md) \(Amazon VPC\)\. | April 29, 2020 | 
| New feature | Amazon SES is now available in [three additional AWS Regions](regions.md): Canada \(Central\), Europe \(London\), and South America \(São Paulo\)\. | April 1, 2020 | 
| New feature | You can now use your own IP ranges to send email\. For more information, see [Using Your Own IP Addresses to Send Email Using Amazon SES](dedicated-ip-byo.md)\. | December 23, 2019 | 
| New feature | You can now use your own public\-private key pair to complete the DKIM authentication process for a domain\. For more information, see [Provide Your Own DKIM Authentication Token in Amazon SES](send-email-authentication-dkim-bring-your-own.md)\. | December 13, 2019 | 
| New feature | You can now use an [account\-level suppression list](sending-email-suppression-list.md) to automatically prevent sending messages to email addresses that previously resulted in a bounce or complaint\. | November 25, 2019 | 
| New feature | If your account is in good standing, and you're approaching the sending quotas for your account, Amazon SES will automatically increase your quotas\. For more information, see [Increasing your Amazon SES sending quotas](manage-sending-quotas-request-increase.md)\. |  | 
| Documentation update | Added [information about deleting personal data from Amazon SES](deleting-personal-data.md)\. | March 13, 2018 | 
| Open sourced documentation | The Amazon SES documentation is now available on [GitHub](https://github.com/awsdocs/amazon-ses-developer-guide)\. You can submit issues or request changes in the GitHub repository, or make changes directly and submit a pull request\. | February 22, 2018 | 
| Documentation update | Added a section that provides information about [deleting personal data](deleting-personal-data.md) stored in Amazon SES\. | February 28, 2018 | 
| Documentation update | Revised the [Amazon SNS event publishing field definitions](event-publishing-retrieving-sns-contents.md), and added a [Rendering Failure event example](event-publishing-retrieving-sns-examples.md#event-publishing-retrieving-sns-failure)\. | January 22, 2018 | 
| Documentation update | Updated Deliverability Dashboard appendix to account for changes to IAM and Lambda consoles\.  We removed this appendix on May 3, 2019, because it used components that were no longer supported\.   | January 18, 2018 | 
| Documentation update | Updated [content related to publishing events to CloudWatch](event-publishing-add-event-destination-cloudwatch.md) to mention blocked fields\. | January 15, 2018 | 
| Documentation update | Updated [procedures for sending email using OpenSSL](send-email-smtp-client-command-line.md) to make them easier to follow\. | January 11, 2018 | 
| Documentation update | Added code example for sending raw email by using the AWS SDK for Ruby\. | January 2, 2018 | 
| Documentation update | Added code example for sending raw email by using the AWS SDK for PHP\. | December 29, 2017 | 
|  New feature  |  Added content related to custom verification emails\.  |  December 7, 2017  | 
|  New feature  |  Added content related to pausing email sending and exporting reputation metrics for configuration sets\.  |  November 15, 2017  | 
| Documentation update | Added code example for sending raw email by using the AWS SDK for Java\. | October 23, 2017 | 
| Documentation update | Added code example for sending raw email by using the AWS SDK for Python \(Boto\)\. | October 20, 2017 | 
| New feature | Added content related to the email templates and personalized email features\. | October 11, 2017 | 
| New feature | Added content related to the open and click custom domain feature\. | September 18, 2017 | 
| New feature | Added content related to the reputation dashboard\. | August 24, 2017 | 
|  New feature  |  Added content related to dedicated IP pools feature\.  |  August 17, 2017  | 
|  New feature  |  Added content related to open and click tracking feature\.  |  August 1, 2017  | 
| Documentation update | Added an index of code examples\. | June 26, 2017 | 
| Documentation update | Added an appendix that demonstrates the process of creating a deliverability dashboard for Amazon SES\.  We removed this appendix on May 3, 2019, because it used components that were no longer supported\.   | June 22, 2017 | 
| Documentation update | Updated email sending code examples\. | June 6, 2017 | 
|  New feature  |  Updated for dedicated IPs\.  |  November 21, 2016   | 
|  New feature  |  Updated for email sending event publishing\.  |  November 2, 2016   | 
|  Service update  |  Updated to reflect that users no longer need to explicitly enable Easy DKIM signing after generating their DKIM records\.  |  September 15, 2016   | 
|  Documentation update  |  Added a getting started tutorial for receiving email\.  |  July 12, 2016   | 
|  New feature  |  Updated for enhanced notifications\.  |  June 14, 2016   | 
|  New feature  |  Updated for custom MAIL FROM domains\.  |  March 14, 2016   | 
|  New feature  |  Updated for inbound email\.  |  September 28, 2015   | 
|  New feature  |  Updated for sending authorization\.  |  July 8, 2015   | 
|  New feature  |  Updated for AWS CloudTrail logging\.  |  May 7, 2015   | 
|  Service update  |  Updated to reflect the consolidation of the Amazon SES quotas increase forms and removed "production access" terminology\.  |  April 8, 2015   | 
|  Service update  |  Updated with new requirements for domain verification TXT records\.  |  February 25, 2015   | 
|  Documentation update  |  Added Enforcement FAQ\.  |  December 15, 2014   | 
|  New feature  |  Updated for delivery notifications\.  |  June 23, 2014   | 
|  New feature  |  Updated for subdomain support\.  |  March 19, 2014   | 
|  New feature  |  Updated for Amazon SES expansion to the US West \(Oregon\) region\.  |  January 29, 2014   | 
|  New feature  |  Updated for Amazon SES expansion to the Europe \(Ireland\) region\.  |  January 15, 2014   | 
|  New feature  |  Updated to reflect the changes in validation of Header Fields and MIME Types\.  |  November 6, 2013   | 
|  Documentation update  |  Removed content on Sender ID\.  |  August 22, 2013   | 
|  New feature  |  Updated to reflect the Amazon SES console redesign\.  |  June 19, 2013   | 
|  New feature  |  Renamed the blacklist feature to global suppression list\.  |  May 8, 2013   | 
|  New feature  |  Updated for the global suppression list removal feature\.  |  March 4, 2013   | 
|  Documentation update  |  Added MIME types\.  |  February 4, 2013   | 
|  Documentation update  |  Included a Getting Started section to replace the stand\-alone Getting Started guide, restructured the Table of Contents, and updated the Sendmail integration instructions\.  |  January 21, 2013   | 
|  Documentation update  |  Added troubleshooting sections on increasing throughput and SMTP issues\.  |  December 12, 2012   | 
|  Documentation update  |  Restructured the information on sending quotas\.  |  November 9, 2012   | 
|  New feature  |  Updated for the Amazon SES mailbox simulator\.   |  October 3, 2012   | 
|  New feature  |  Updated for using a DKIM signature to sign email from a verified identity\.  |  July 17, 2012  | 
|  New feature  |  Updated for receiving bounce and complaint feedback notifications through Amazon Simple Notification Service \(Amazon SNS\)\.  |  June 26, 2012  | 
|  New feature  |  Updated for domain verification\.  |  May 15, 2012  | 
|  New feature  |  Updated to reflect additional header and attachment types\.  |  April 25, 2012  | 
|  New feature  |  Updated for the STARTTLS extension to SMTP\.  |  March 7, 2012  | 
|  New feature  |  Updated for Variable Envelope Return Path \(VERP\)\.  |  February 22, 2012  | 
|  New feature  |  Updated for SMTP support\.  |  December 13, 2011  | 
|  New feature  |  Updated for AWS Management Console support\.  |  November 17, 2011  | 
|  New feature  |  Updated for attachment support\.  |  July 18, 2011  | 
|  Initial release  |  This is the first release of the *Amazon Simple Email Service Developer Guide*\.  |  January 25, 2011  | 


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 