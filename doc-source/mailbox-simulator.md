# Testing Email Sending in Amazon SES<a name="mailbox-simulator"></a>

Amazon SES includes a mailbox simulator that you can use to test how your application handles different email sending scenarios\. The mailbox simulator is useful when, for example, you need to test an email sending application without creating fictitious email addresses, or when you need to find your system's maximum throughput without impacting your daily sending quota\.

## Important Considerations<a name="mailbox-simulator-considerations"></a>

Consider the following features and limitations when you use the Amazon SES mailbox simulator:
+ You can use the mailbox simulator even if your account is in the Amazon SES sandbox\.
+ Emails that you send to the mailbox simulator are limited by your account's maximum sending rate, but they don't affect your daily sending limits\. For example, if your account is authorized to send 10,000 messages per 24\-hour period, and you send 100 messages to the mailbox simulator, you can still send up to 10,000 messages to regular recipients without reaching your sending limit\.
+ Emails that you send to the mailbox simulator don't impact your email deliverability or reputation metrics\. For example, if you send a large number of messages to the bounce address of the email simulator, it doesn't cause the [reputation dashboard](reputation-dashboard-dg.md) to display a message warning you that your bounce rate is too high\.
+ For billing purposes, emails that you send to the Amazon SES mailbox simulator are the same as any other email you send using Amazon SES\. In other words, we bill you the same amount for messages you send to the mailbox simulator as for those you that send to regular recipients\.
+ The mailbox simulator supports labeling, which enables you to send emails to the same mailbox simulator address in multiple ways, or to see how your application handles Variable Envelope Return Path \(VERP\)\. For example, you can send an email to *bounce\+label1@simulator\.amazonses\.com* and *bounce\+label2@simulator\.amazonses\.com* to see if your application can match a bounce message with the email address that caused the bounce\.
+ If you use the mailbox simulator to simulate multiple bounces from the same sending request, Amazon SES combines the bounce responses into a single response\.

## Using the Mailbox Simulator<a name="mailbox-simulator-using"></a>

To use the email simulator, find the scenario that you want to simulate in the following table, and then send an email to the corresponding email address\. 

**Note**  
When you send an email to a mailbox simulator address, you must send it through Amazon SES, by using the AWS CLI, an AWS SDK, the Amazon SES console, the Amazon SES SMTP interface, or the Amazon SES API\. The mailbox simulator doesn't respond to emails that it receives from external sources\.


| Simulated scenario | Email address | 
| --- | --- | 
| Successful delivery—The recipient's email provider accepts your email\. If you set up delivery notifications as described in [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md), Amazon SES sends you a delivery notification through Amazon Simple Notification Service \(Amazon SNS\)\.  | success@simulator\.amazonses\.com | 
| Bounce—The recipient's email provider rejects your email with an SMTP 550 5\.1\.1 \("Unknown User"\) response code\. Amazon SES generates a bounce notification and, depending on how you set up your account, sends it to you in an email or sends a notification to an Amazon SNS topic\. The mailbox simulator email address isn't placed on the Amazon SES suppression list, which would normally happen when a hard bounce occurs\. The bounce response that you receive from the mailbox simulator is compliant with [RFC 3464](https://tools.ietf.org/html/rfc3464)\. For information about how to receive bounce feedback, see [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\.  | bounce@simulator\.amazonses\.com | 
| Automatic responses—The recipient's email provider accepts your email and delivers it to the recipient’s inbox\. The email provider sends an automatic response, such as an "out of the office" \(OOTO\) message, to the address in the Return\-Path header of the email, or the envelope sender \("MAIL FROM"\) address if the Return\-Path header isn't present\. The automatic response that you receive from the mailbox simulator is compliant with [RFC 3834](https://tools.ietf.org/html/rfc3834)\.  | ooto@simulator\.amazonses\.com | 
| Complaint—The recipient's email provider accepts your email and delivers it to the recipient’s inbox\. The recipient decides that your message is unsolicited and clicks "Mark as Spam" in his or her email client\. Amazon SES then forwards the complaint notification to you by email or by notifying an Amazon SNS topic, depending on how you set up your account\. The complaint response that you receive from the mailbox simulator is compliant with [RFC 5965](https://tools.ietf.org/html/rfc5965)\. For information about how to receive complaint feedback, see [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\. | complaint@simulator\.amazonses\.com | 
| Recipient address on suppression list—Amazon SES generates a hard bounce as if the recipient's address is on the Amazon SES suppression list\. | suppressionlist@simulator\.amazonses\.com | 

## Testing Reject Events<a name="mailbox-simulator-reject"></a>

Every message that you send through Amazon SES is scanned for viruses\. If you send a message that contains a virus, Amazon SES accepts the message, detects the virus, and rejects the entire message\. When Amazon SES rejects the message, it stops processing the message, and doesn't attempt to deliver it to the recipient's mail server\. It then generates a Reject event\.

The Amazon SES mailbox simulator doesn't include an address for testing Reject events\. However, you can test Reject events by using an EICAR test file\. This file is an industry\-standard method of testing anti\-virus software in a safe manner\. To create an EICAR test file, paste the following text into a file:

```
X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
```

Save the file as `sample.txt`, attach it to an email, and then send the email to a verified address\. If there are no other issues with the email, Amazon SES accepts the message, but then rejects it as it would if it contained an actual virus\.

**Note**  
Rejected emails—including those that you send by using the procedure above—count against your daily sending quota\. We bill you for each message that you send, including rejected messages\.

To learn more about EICAR test files, see the [EICAR test file page on Wikipedia](https://en.wikipedia.org/wiki/EICAR_test_file)\. For code examples that you can use to send messages with attachments, see [Sending Raw Email using AWS SDKs](examples-send-raw-using-sdk.md)\.


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 