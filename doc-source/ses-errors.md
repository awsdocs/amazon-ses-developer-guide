# Amazon SES Email Sending Errors<a name="ses-errors"></a>

This topic reviews the types of email sending\-specific errors that you may encounter when you send an email through Amazon SES\. If you try to send an email through Amazon SES and the call to Amazon SES fails, Amazon SES returns an error message to your application and does not send the email\. The way that you observe this error message depends on the way that you call Amazon SES\.

+ If you call the Amazon SES API directly, the Query action will return an error\. The error may be `MessageRejected` or one of the errors specified in the [Common Errors](http://tinyurl.com/bfzl8s6) topic of the *Amazon Simple Email Service API Reference*\.

+ If you call Amazon SES using an AWS SDK that uses a programming language that supports exceptions, Amazon SES may throw an exception\. The type of exception depends on the SDK and on the error\. For example, the exception could be an Amazon SES `MessageRejectedException` \(the actual name may vary depending on the SDK\) or a general AWS exception\. Regardless of the type of exception, the error type and the error message in the exception will give you more information\.

+ If you call Amazon SES through its SMTP interface, the way that you experience the error depends on the application\. Some applications may display a specific error message, some may not\. For a list of SMTP response codes, see [SMTP Response Codes Returned by Amazon SES](smtp-response-codes.md)\. 

**Note**  
When your call to Amazon SES to send an email fails, you are not billed for that email\.

The following are the types of Amazon SES\-specific problems that can cause Amazon SES to return an error when you try to send an email\. These errors are in addition to general AWS errors like `MalformedQueryString` as specified in the [Common Errors](http://tinyurl.com/bfzl8s6) topic of the *Amazon Simple Email Service API Reference*\.

+ **Email address is not verified\. The following identities failed the check in region <region>: <identity1>, <identity2>, <identity3>**—You are trying to send email from an email address or domain that you have not [verified with Amazon SES](verify-addresses-and-domains.md)\. This error could apply to the "From", "Source", "Sender", or "Return\-Path" address\. If your account is still in the sandbox, you also must verify every recipient email address except for the recipients provided by the [Amazon SES mailbox simulator](mailbox-simulator.md)\. If Amazon SES is not able to show all of the failed identities, the error message ends with an ellipsis\.
**Note**  
Amazon SES has endpoints in [multiple AWS regions](regions.md), and email address verification status is separate for each AWS region\. You must complete the verification process for each sender in the AWS region\(s\) you want to use\.

+ **Customer is suspended**—Your AWS account has been blocked from sending email using Amazon SES\. You can still access the Amazon SES console and perform any activity \(e\.g\., view your metrics\) except for email sending; if you attempt to send an email, you will receive this error message\. 

  If this happens, you should have received an email from Amazon SES to the email address associated with your AWS account informing you of the problem\. To appeal your suspension and reinstate email sending privileges, follow the instructions in the email\. You will need to explain in detail why you believe that the suspension itself was an error, or the changes you have made to ensure that the same problem does not occur again\.

+ **Throttling**—Amazon SES is limiting the rate at which you can send messages\. Your application may be trying to send too much email, or to send email at too fast a rate\. In these cases, the error may be similar to the following:

  + **Daily message quota exceeded**—You have sent the maximum number of messages that you are permitted in a 24\-hour period\. If you have exceeded your daily quota, you will have to wait until the next 24\-hour period before you can send more email\.

  + **Maximum sending rate exceeded**—You are attempting to send more emails per second than is permitted by your maximum send rate\. If you have exceeded your sending rate, you can continue to send email, but will need to reduce your send rate\. For more information, see [How to handle a "Throttling \- Maximum sending rate exceeded" error ](https://aws.amazon.com//blogs/ses/how-to-handle-a-throttling-maximum-sending-rate-exceeded-error/) on the Amazon SES blog\.

  You should regularly monitor your sending activity to see how close you are to your sending limits\. For more information, see [Monitoring Your Amazon SES Sending Limits](monitor-sending-limits.md)\. For general information about sending limits, see [Managing Your Amazon SES Sending Limits](manage-sending-limits.md)\. For information about how to increase your sending limits, see [Increasing Your Amazon SES Sending Limits](increase-sending-limits.md)\.
**Important**  
If the error text that explains the throttling error is not related to you exceeding your daily quota or maximum send rate, then there might be a system\-wide problem that is causing reduced sending capabilities\. For information about the service status, go to the [AWS Service Health Dashboard](http://status.aws.amazon.com/)\.

+ **There are no recipients specified**—No recipients were provided\. 

+ **There are non\-ASCII characters in the email address**—The email address string must be 7\-bit ASCII\. If you want to send to or from email addresses that contain Unicode characters in the domain part of an address, you must encode the domain using Punycode\. Punycode is not permitted in the local part of the email address \(i\.e\., the part before the @\) nor in the "friendly from" name\. If you want to use Unicode characters in the "friendly from" name, you must encode the "friendly from" name using MIME encoded\-word syntax, as described in [Sending Raw Email Using the Amazon SES API](send-email-raw.md)\. For more information about Punycode, see [RFC 3492](http://tools.ietf.org/html/rfc3492)\. 

+ **Mail FROM domain is not verified**—Amazon SES could not read the MX record required to use the specified MAIL FROM domain\. For information about editing the custom MAIL FROM domain settings for an identity, see [Editing a MAIL FROM Domain with Amazon SES](mail-from-edit.md)\. 

+ **Configuration set does not exist**—The configuration set that you specified does not exist\. A configuration set is an optional parameter that you use to publish email sending events\. For more information, see [Monitoring Using Amazon SES Event Publishing](monitor-using-event-publishing.md)\. 