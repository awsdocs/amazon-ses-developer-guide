# Regions and Amazon SES<a name="regions"></a>

Amazon SES is available in several AWS Regions in North America and Europe\. In each Region, AWS maintains multiple Availability Zones\. These Availability Zones are physically isolated from each other, but are united by private, low\-latency, high\-throughput, and highly redundant network connections\. These Availability Zones enable us to provide very high levels of availability and redundancy, while also minimizing latency\.

For a list of all of the Regions where Amazon SES is currently available, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *Amazon Web Services General Reference*\. To learn more about the number of Availability Zones that are available in each Region, see [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)\.

This section contains information that you need to know if you plan to use Amazon SES in multiple AWS Regions\. It discusses the following subjects:
+ [Amazon SES Regions and Endpoints](#region-endpoints)
+ [Sandbox and Sending Limit Increases](#region-limit-increases)
+ [Verification of Email Addresses and Domains](#region-verification)
+ [Easy DKIM](#region-dkim)
+ [Suppression List](#region-suppression-list)
+ [Feedback Notifications](#region-feedback-notifications)
+ [SMTP Credentials](#region-smtp)
+ [Sending Authorization](#region-sending-authorization)
+ [Custom MAIL FROM Domains](#region-mail-from)
+ [Email Receiving](#region-receive-email)

For general information about AWS Regions, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *AWS General Reference\.*

## Amazon SES Regions and Endpoints<a name="region-endpoints"></a>

When you use Amazon Simple Email Service \(Amazon SES\) to send email, you connect to a URL that provides an endpoint for the Amazon SES API or SMTP interface\. The *AWS General Reference* contains a complete list of endpoints that you use to send and receive email through Amazon SES\. For more information, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

When you send email through Amazon SES, you can use the URLs in the **API \(HTTPS\) Endpoint** column to make HTTPS requests to the Amazon SES API\. You can also use the URLs in the **SMTP Endpoint** column to send email by using the SMTP interface\.

If you've configured Amazon SES to receive email that's sent to your domain, you can use the inbound SMTP endpoint URLs \(that is, the URLs that begin with "inbound\-smtp\."\) when you [set up the mail exchanger \(MX\) records in the DNS settings for your domain](receiving-email-mx-record.md)\.

**Note**  
The inbound SMTP URLs aren't IMAP server addresses\. In other words, you can't use them to receive email by using an application such as Outlook\. For a service that provides an IMAP server for incoming email, see [Amazon WorkMail](https://aws.amazon.com/workmail)\.

## Sandbox and Sending Limit Increases<a name="region-limit-increases"></a>

The sandbox status for your account can differ between AWS Regions\. In other words, if your account has been removed from the sandbox in the US West \(Oregon\) Region, it might still be in the sandbox in the US East \(N\. Virginia\) Region, unless you've also had it removed from the sandbox in that Region\.

Sending limits can also be different depending on the AWS Region\. For example, if your account is able to send 10 messages per second in the EU \(Ireland\) Region, you might be able to send more or fewer messages in other Regions\.

When you [submit a request to have your account removed from the sandbox](request-production-access.md), or when you [submit a request to have your account's sending limits increased](submit-extended-access-request.md), be sure to choose all of the AWS Regions that your request applies to\. You can submit several requests in a single Support Center case\.

## Verification of Email Addresses and Domains<a name="region-verification"></a>

Before you can send email using Amazon SES, you have to verify that you own the email address or domain that you plan to send from\. The verification status of email addresses and domains also differs across AWS Regions\. For example, if you verify a domain in the US West \(Oregon\) Region, you can't use that domain to send email in the US East \(N\. Virginia\) Region until you complete the verification process again for that Region\. For more information about verifying email addresses and domains, see [Verifying Identities in Amazon SES](verify-addresses-and-domains.md)\.

## Easy DKIM<a name="region-dkim"></a>

You have to perform the Easy DKIM setup process for each Region where you want to use Easy DKIM\. That is, in each Region, you have to use the Amazon SES console or the Amazon SES API to generate TXT records\. Next, you have to add all of the TXT records to the DNS configuration for your domain\. For more information about setting up Easy DKIM, see [Easy DKIM in Amazon SES](easy-dkim.md)\.

## Suppression List<a name="region-suppression-list"></a>

Although each Region has a separate suppression list, if you remove an address from the suppression list of one Region, the address is removed from the suppression list of all Regions\. You remove addresses from the suppression list by using the Amazon SES console\. For more information about removing addresses from the suppression list, see [Removing an Email Address from the Amazon SES Suppression List](remove-from-suppression-list.md)\.

## Feedback Notifications<a name="region-feedback-notifications"></a>

There are two important points to note about setting up feedback notifications in multiple Regions:
+ Verified identity settings, such as whether you receive feedback by email or through Amazon Simple Notification Service \(Amazon SNS\), only apply to the Region that you set them in\. For example, if you verify *user@example\.com* in the US West \(Oregon\) and US East \(N\. Virginia\) Regions and you want to receive bounced emails via Amazon SNS notifications, you have to use the Amazon SES API or the Amazon SES console to set up Amazon SNS feedback notifications for *user@example\.com* in both Regions\.
+ Amazon SNS topics that you use for feedback forwarding have to be in the same Region where you use Amazon SES\.

## SMTP Credentials<a name="region-smtp"></a>

The credentials that you use to send email through the Amazon SES SMTP interface are unique to each AWS Region\. If you use the Amazon SES SMTP interface to send email in more than one Region, you have to [generate a set of SMTP credentials](smtp-credentials.md) for each Region\.

**Note**  
If you created SMTP credentials before January 10, 2019, your SMTP credentials might work in all AWS Regions where Amazon SES is available\. However, credentials created after this date are created using the AWS Signature Version 4, and are unique to each Region\.  
For additional security, we recommend that you delete credentials that were created before this date, and replace them with newer, Region\-specific credentials\. You can [delete older credentials by using the IAM console](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_manage.html#id_users_deleting)\.

## Custom MAIL FROM Domains<a name="region-mail-from"></a>

You can use the same custom MAIL FROM domain for verified identities in different AWS Regions\. If that is what you want to do, you only need to publish one MX record to the MAIL FROM domain's DNS server\. In this situation, bounce notifications are sent to the Amazon SES feedback endpoint in the Region that you specified in the MX record first\. Next Amazon SES redirects the bounces to the verified identity in the Region that sent the email\.

Use the MX record settings that Amazon SES provides during the custom MAIL FROM setup process for an identity in one of the Regions\. The custom MAIL FROM setup process is described in [Setting a MAIL FROM Domain](mail-from-set.md)\. For reference, you can find the feedback endpoints for all of the Regions in the following table\.


****  

| Region Name | Feedback Endpoints for Custom MAIL FROM Sending Configurations | 
| --- | --- | 
|  US East \(N\. Virginia\)  |  feedback\-smtp\.us\-east\-1\.amazonses\.com  | 
|  US West \(Oregon\)  |  feedback\-smtp\.us\-west\-2\.amazonses\.com  | 
|  EU \(Ireland\)  |  feedback\-smtp\.eu\-west\-1\.amazonses\.com  | 

## Sending Authorization<a name="region-sending-authorization"></a>

Delegate senders can only send emails from the AWS Region where the identity owner's identity is verified\. The sending authorization policy that gives permission to the delegate sender must be attached to the identity in that Region\. For more information about sending authorization, see [Using Sending Authorization with Amazon SES](sending-authorization.md)\.

## Email Receiving<a name="region-receive-email"></a>

When you receive email with Amazon SES, all of the resources that you use must be in the same Region as the Amazon SES endpoint\.

For example, if you use the Amazon SES endpoint in US West \(Oregon\), then any Amazon S3 bucket, Amazon SNS topic, AWS KMS key, and Lambda function that you use must also be in US West \(Oregon\)\. Similarly, to receive mail with Amazon SES within a Region, you must have an active receipt rule set within that Region\.


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 