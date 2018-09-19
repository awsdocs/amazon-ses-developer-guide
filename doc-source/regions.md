# Regions and Amazon SES<a name="regions"></a>

When you use Amazon Simple Email Service \(Amazon SES\), you connect to a URL that provides an endpoint for the Amazon SES API or SMTP interface\. Amazon SES has endpoints in multiple AWS regions\. To reduce network latency, it's a good idea to choose an endpoint closest to your application\.

This topic contains information you need to know when you use Amazon SES endpoints in multiple AWS regions\. It discusses the following subjects:
+ [Amazon SES Endpoints](#region-endpoints)
+ [Selecting a Region to Use with Amazon SES](#region-select)
+ [Sandbox and Sending Limit Increases](#region-limit-increases)
+ [Verification](#region-verification)
+ [Easy DKIM Setup](#region-dkim)
+ [Suppression List](#region-suppression-list)
+ [Feedback Notifications](#region-feedback-notifications)
+ [SMTP Credentials](#region-smtp)
+ [Sending Authorization](#region-sending-authorization)
+ [Custom MAIL FROM Domains](#region-mail-from)
+ [Email Receiving](#region-receive-email)

For general information about AWS regions, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *AWS General Reference\.*

## Amazon SES Endpoints<a name="region-endpoints"></a>

The following sections list the AWS regions in which Amazon SES is available, and the corresponding endpoints for sending and receiving emails\.

### Email Sending Endpoints<a name="region-endpoints-sending"></a>

The following table lists the endpoints you use for email sending\.


****  

| Region name | Region | API \(HTTPS\) endpoint | SMTP endpoint | 
| --- | --- | --- | --- | 
|  US East \(N\. Virginia\)  | us\-east\-1 |  email\.us\-east\-1\.amazonaws\.com  |  email\-smtp\.us\-east\-1\.amazonaws\.com  | 
|  US West \(Oregon\)  | us\-west\-2 |  email\.us\-west\-2\.amazonaws\.com  |  email\-smtp\.us\-west\-2\.amazonaws\.com  | 
|  EU \(Ireland\)  | eu\-west\-1 |  email\.eu\-west\-1\.amazonaws\.com  |  email\-smtp\.eu\-west\-1\.amazonaws\.com  | 

### Email Receiving Endpoints<a name="region-endpoints-receiving"></a>

The following table lists the endpoints you use for email receiving\.


****  

| Region name | Region | SMTP endpoint | 
| --- | --- | --- | 
|  US East \(N\. Virginia\)  | us\-east\-1 |  inbound\-smtp\.us\-east\-1\.amazonaws\.com  | 
|  US West \(Oregon\)  | us\-west\-2 |  inbound\-smtp\.us\-west\-2\.amazonaws\.com  | 
|  EU \(Ireland\)  | eu\-west\-1 |  inbound\-smtp\.eu\-west\-1\.amazonaws\.com  | 

## Selecting a Region to Use with Amazon SES<a name="region-select"></a>

The following sections describe how to select a region depending on which method you use to call Amazon SES\.

### Amazon SES API<a name="region-select-api"></a>

When you use the Amazon SES API, you specify an endpoint in the Query request\. That endpoint determines the AWS region you are using\. For more information, see [Query Requests and Amazon SES](query-interface-requests.md)\.

### Amazon SES SMTP Interface<a name="region-select-smtp"></a>

The SMTP interface is for email sending only\. When you use the SMTP interface, the SMTP endpoint you specify in your code or configuration settings determines the AWS region you are using\. For more information, see [Connecting to the Amazon SES SMTP Endpoint](smtp-connect.md)\.

### Amazon SES Console<a name="region-select-console"></a>

When you use the Amazon SES console, you can change the endpoint by clicking the region name in the upper right corner of the navigation bar, as shown in the following screenshot\.

![\[Region selector\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/region-selector.png)

## Sandbox and Sending Limit Increases<a name="region-limit-increases"></a>

Sandbox status and sending limits apply on a per\-region basis\. You must request sending limit increases for each region individually\. When you open an [SES Sending Limits Increase case](https://aws.amazon.com/ses/extendedaccessrequest/) in Support Center, the form has a menu you use to select the AWS region for which you are requesting a sending limit increase\. For more information on increasing your sending limits, see [Opening an SES Sending Limits Increase Case](submit-extended-access-request.md)\.

## Verification<a name="region-verification"></a>

Before you send email using Amazon SES, you must verify that you own your email address or domain with Amazon SES\. Verification status for each region is separate, as described in the following sections\.

### Email Address Verification<a name="region-email-address-verification"></a>

You must verify each sender's email address separately for each region you want to use\. For example, if you verify an email address in the US West \(Oregon\) region, you will be able to send from it when you connect to an Amazon SES endpoint in the US West \(Oregon\) region, but you will not be able to send from it using an endpoint in the US East \(N\. Virginia\) region until you verify that email address in the US East \(N\. Virginia\) region\. For more information about verifying email addresses, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)\.

### Domain Verification<a name="region-domain-verification"></a>

Like email address verification, domain verification applies to each region separately\. You must perform the domain verification procedure for each region in which you want to send from a given domain\. For example, if you want to send email from *example\.com* from both the US West \(Oregon\) region endpoint and the US East \(N\. Virginia\) region endpoint, you must add two TXT records to your DNS settings — one record for each region\. You generate these records by using the Amazon SES console with the appropriate region selected, or the Amazon SES API endpoint that corresponds to the region you want\. For more information about verifying domains, see [Verifying Domains in Amazon SES](verify-domains.md)\.

## Easy DKIM Setup<a name="region-dkim"></a>

You must perform the Easy DKIM setup procedure for each region in which you want to use Easy DKIM\. That is, for each region, you must use the Amazon SES console or the Amazon SES API to generate TXT records, add the TXT records to your DNS settings, and then use the Amazon SES API or the Amazon SES console to enable DKIM signing for your chosen sending identity \(email address or domain\) within that region\. For more information about setting up Easy DKIM, see [Easy DKIM in Amazon SES](easy-dkim.md)\.

## Suppression List<a name="region-suppression-list"></a>

Although each region has a separate suppression list, if you remove an address from the suppression list of one region, the address is removed from the suppression list of all regions\. You remove addresses from the suppression list by using the Amazon SES console\. For more information about the suppression list, see [Removing an Email Address from the Amazon SES Suppression List](remove-from-suppression-list.md)\.

## Feedback Notifications<a name="region-feedback-notifications"></a>

There are two important points to note about setting up feedback notifications in multiple regions:
+ Verified identity settings, such as whether you receive feedback by email or through Amazon Simple Notification Service \(Amazon SNS\), apply only to the region in which you set them\. For example, if you verify user@example\.com in the US West \(Oregon\) and US East \(N\. Virginia\) regions and you want to receive bounced emails via Amazon SNS notifications, you must use the Amazon SES API or the Amazon SES console to set up Amazon SNS feedback notifications for user@example\.com in both regions\.
+ Amazon SNS topics you use for feedback forwarding must be within the same region in which you are using Amazon SES\.

## SMTP Credentials<a name="region-smtp"></a>

You can use the same set of SMTP credentials in all regions\. For more information about SMTP credentials, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.

## Custom MAIL FROM Domains<a name="region-mail-from"></a>

You can use the same custom MAIL FROM domain for verified identities in different AWS regions\. If that is what you want to do, you must still publish only one MX record to the MAIL FROM domain's DNS server\. Bounces returned by ISPs will go to the Amazon SES feedback endpoint in the region specified in the MX record first, and then Amazon SES will redirect the bounces to the verified identity in the region that sent the email\.

Use the MX record settings that Amazon SES provides during the custom MAIL FROM setup for an identity in one of the regions\. The custom MAIL FROM setup process is described in [Setting a MAIL FROM Domain](mail-from-set.md)\. For reference, you can find the feedback endpoints for all of the regions in the following table\.


****  

| Region name | Feedback endpoints for custom MAIL FROM sending configurations | 
| --- | --- | 
|  US East \(N\. Virginia\)  |  feedback\-smtp\.us\-east\-1\.amazonses\.com  | 
|  US West \(Oregon\)  |  feedback\-smtp\.us\-west\-2\.amazonses\.com  | 
|  EU \(Ireland\)  |  feedback\-smtp\.eu\-west\-1\.amazonses\.com  | 

## Sending Authorization<a name="region-sending-authorization"></a>

The delegate sender must send the emails from the AWS region in which the identity owner's identity is verified\. The sending authorization policy that gives permission to the delegate sender must be attached to the identity in that region\. For more information about sending authorization, see [Using Sending Authorization with Amazon SES](sending-authorization.md)\.

## Email Receiving<a name="region-receive-email"></a>

When you receive email with Amazon SES, all of the resources that you use must be in the same region as the Amazon SES endpoint\.

**Note**  
For a list of endpoints for Amazon SES email receiving, see [Email Receiving Endpoints](#region-endpoints-receiving)\.

For example, if you use the Amazon SES endpoint in US West \(Oregon\), then any Amazon S3 bucket, Amazon SNS topic, AWS KMS key, and Lambda function that you use must also be in US West \(Oregon\)\. Similarly, to receive mail with Amazon SES within a region, you must have an active receipt rule set within that region\.


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, visit the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 