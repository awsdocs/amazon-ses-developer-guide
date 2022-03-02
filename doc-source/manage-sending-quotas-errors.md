# Errors related to the sending quotas for your Amazon SES account<a name="manage-sending-quotas-errors"></a>

If you attempt to send an email after reaching your daily sending quota \(the maximum amount of email you can send in a 24\-hour period\) or your maximum sending rate \(the maximum number of messages you can send per second\), Amazon SES drops the message and doesn't attempt to redeliver it\. Amazon SES also provides an error message that explains the issue\. The way that Amazon SES produces this error message depends on how you attempted to send the email\. This topic includes information about the messages you receive through the Amazon SES API and through the SMTP interface\. 

For a technique that you can use when you reach your maximum send rate, see [ How to handle a "Throttling – Maximum sending rate exceeded" error](https://aws.amazon.com//blogs/messaging-and-targeting/how-to-handle-a-throttling-maximum-sending-rate-exceeded-error/) on the AWS Messaging and Targeting Blog\.

## Reaching sending limits with the Amazon SES API<a name="manage-sending-quotas-errors-api"></a>

If you attempt to send an email by using the Amazon SES API \(or an AWS SDK\), but you've already exceeded your account's sending limits, the API produces a `ThrottlingException` error\. The error message includes one of the following messages:
+ `Daily message quota exceeded`
+ `Maximum sending rate exceeded`

If you encounter a throttling error, you should program your application to wait for an interval of up to 10 minutes, and then retry the send request\.

## Reaching sending limits with SMTP<a name="manage-sending-quotas-errors-smtp"></a>

If you attempt to send an email by using the Amazon SES SMTP interface, but you've already exceeded your account's sending limits, your SMTP client might display one of the following errors:
+ `454 Throttling failure: Maximum sending rate exceeded`
+ `454 Throttling failure: Daily message quota exceeded`

Different SMTP clients handle these errors in different ways\.