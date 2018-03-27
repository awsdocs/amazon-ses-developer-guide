# What Happens When You Reach Your Amazon SES Sending Limits<a name="reach-sending-limits"></a>

If you attempt to send an email after you have reached your sending quota or maximum send rate, you will encounter a throttling error and the email will be dropped\. The way you observe the error depends on whether you are calling the Amazon SES API directly, or accessing Amazon SES through the SMTP interface\. The following sections describe both scenarios\.

**Note**  
You can send an email to multiple recipients as long as you have at least one email left before you reach your sending rate limit\. Then, you have to wait until you build up the corresponding amount of sending rate quota before you can send again\. For example, if your sending rate limit is one email per second, then you can send an email with 10 recipients once every 10 seconds\. However, we do not recommend that you send an email to multiple recipients in one call to `SendEmail`\.

For a technique to use when you reach your maximum send rate, see [ How to handle a "Throttling – Maximum sending rate exceeded" error](https://aws.amazon.com//blogs/ses/how-to-handle-a-throttling-maximum-sending-rate-exceeded-error/) on the Amazon SES blog\.

## Reaching Sending Limits with the Amazon SES API<a name="reach-sending-limits-api"></a>

If your application attempts to send an email beyond your sending limits, the application will encounter a [Throttling](http://docs.aws.amazon.com/ses/latest/APIReference/CommonErrors.html) error with one of the following messages:
+ Daily message quota exceeded
+ Maximum sending rate exceeded

A throttling error might occur because of incorrect predictions of email volume, or bursts of transactional email that are higher than expected\. To handle a throttling error, program your application to wait for a random interval of between 0 and 10 minutes, and then retry the send request\.

## Reaching Sending Limits with SMTP<a name="reach-sending-limits-smtp"></a>

If you reach your sending limits when you are accessing Amazon SES through the SMTP interface, your SMTP client will receive one of the following errors:
+ `454 Throttling failure: Maximum sending rate exceeded`
+ `454 Throttling failure: Daily message quota exceeded`

However, SMTP clients handle these errors in various ways\. For example, if you use Microsoft Outlook as your email client, and you attempt to send past your sending quota, you get a Send/Receive error that displays the following text in the status pane at the bottom of the Outlook client window:

`Task 'andrew@example.net- Sending' reported error (0x800CCC7F): 'Establishing an encrypted connection to your outgoing (SMTP) server failed. If this problem continues, contact your server administrator or Internet service provider (ISP). The server responded: 454 Throttling failure: Daily message quota exceeded.'`

The way in which these errors are handled depends on the SMTP client that you use; some SMTP clients may not display the error code at all\.