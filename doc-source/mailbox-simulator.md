# Testing Amazon SES Email Sending<a name="mailbox-simulator"></a>

Amazon Simple Email Service \(Amazon SES\) includes a mailbox simulator that you can use to test how your application handles various email sending scenarios without affecting your sending quota or your bounce and complaint metrics\. You can send emails to the mailbox simulator even if you are in the sandbox\. 

The Amazon SES mailbox simulator is a set of test email addresses\. Each email address represents a specific scenario\. You can send emails to the mailbox simulator when you want to:
+ Test your application without having to create test "To" addresses\.
+ Test how your email sending program handles bounces, complaints, and out\-of\-the\-office \(OOTO\) responses\.
+ See what happens when you email an address that is on the Amazon SES suppression list\.
+ Generate a bounce without putting a valid email address on the suppression list\. 
+ Find your system's maximum throughput without using up your daily sending quota\. 
+ Send test emails without affecting your email deliverability metrics for bounces and complaints\. 

To use the mailbox simulator, email the addresses and observe how your setup responds to the simulated scenarios\. The following table lists each simulated scenario and the corresponding email address that you would use\. The email addresses are not case\-sensitive\.

**Note**  
You can only access the mailbox simulator by using Amazon SES\. You cannot access it from an external mail server\.


****  

| Simulated scenario | Mailbox simulator email address | 
| --- | --- | 
| Success—The recipient's ISP accepts your email\. If you have set up delivery notifications as described in [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md), Amazon SES sends you a delivery notification through Amazon Simple Notification Service \(Amazon SNS\)\. Otherwise, you will not receive any confirmation about this successful delivery other than the API return value\.  | success@simulator\.amazonses\.com | 
| Bounce—The recipient's ISP rejects your email with an SMTP 550 5\.1\.1 response code \("Unknown User"\)\. Amazon SES generates a bounce notification and sends it to you via email or by using an Amazon SNS notification, depending on how you set up your system\. This mailbox simulator email address will not be placed on the Amazon SES suppression list as one normally would when an email hard bounces\. The bounce response that you receive from the mailbox simulator is compliant with [RFC 3464](https://tools.ietf.org/html/rfc3464)\. For information about how to receive bounce feedback, see [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\.  | bounce@simulator\.amazonses\.com | 
| Out of the Office—The recipient's ISP accepts your email and delivers it to the recipient’s inbox\. The ISP sends an out\-of\-the\-office \(OOTO\) message to Amazon SES\. Amazon SES then forwards the OOTO message to you via email or by using an Amazon SNS notification, depending on how you set up your system\. The OOTO response that you receive from the Mailbox Simulator is compliant with [RFC 3834](https://tools.ietf.org/html/rfc3834)\. For information about how to set up your system to receive OOTO responses, follow the same instructions for setting up how Amazon SES sends you notifications in [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\. | ooto@simulator\.amazonses\.com | 
| Complaint—The recipient's ISP accepts your email and delivers it to the recipient’s inbox\. The recipient, however, does not want to receive your message and clicks "Mark as Spam" within an email application that uses an ISP that sends a complaint response to Amazon SES\. Amazon SES then forwards the complaint notification to you via email or by using an Amazon SNS notification, depending on how you set up your system\. The complaint response that you receive from the mailbox simulator is compliant with [RFC 5965](https://tools.ietf.org/html/rfc5965)\. For information about how to receive bounce feedback, see [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\. | complaint@simulator\.amazonses\.com | 
| Address on Suppression List—Amazon SES treats your email as a hard bounce because the address you are sending to is on the Amazon SES suppression list\. | suppressionlist@simulator\.amazonses\.com | 

**Important**  
If you send an email to a mailbox simulator address other than the test addresses listed above, the unlisted address will be placed on the suppression list\.

The mailbox simulator provides typical bounce, complaint, and OOTO responses\. In the bounce scenario, multiple bounces from the same sending request are gathered into a single response\. In practice, the response varies by ISP\. To reduce your bounce and complaint rates, see the [Amazon SES Email Sending Best Practices whitepaper](https://media.amazonwebservices.com/AWS_Amazon_SES_Best_Practices.pdf)\.

When you send emails to the mailbox simulator, you will be limited by your maximum send rate\. You will also be billed for your emails\. However, emails to the mailbox simulator will not affect your email deliverability metrics for bounces and complaints or count against your sending quota\.

The mailbox simulator supports labeling, which enables you to send emails to the same mailbox simulator address in multiple ways, or to test your support for Variable Envelope Return Path \(VERP\)\. For example, you can send an email to bounce\+label1@simulator\.amazonses\.com and bounce\+label2@simulator\.amazonses\.com to test how your setup matches a bounce message with the undeliverable address that caused the bounce\. For more information about VERP, see [https://en\.wikipedia\.org/wiki/Variable\_envelope\_return\_path](https://en.wikipedia.org/wiki/Variable_envelope_return_path)\.

## Testing Reject Events<a name="mailbox-simulator-reject"></a>

Every message that is sent through Amazon SES is scanned for viruses\. If you send a message that contains a virus, Amazon SES accepts the message, detects the virus, and rejects the entire message\. When a message is rejected, Amazon SES stops processing it, and doesn't attempt to deliver it to the recipient's mail server\. Amazon SES also generates a Reject event when a message is rejected\.

The Amazon SES mailbox simulator doesn't include an address for testing Reject events\. However, you can test Reject events by using an EICAR test file\. This file is an industry\-standard method of testing anti\-virus software in a completely safe manner\. To create an EICAR test file, paste the following text into a file:

```
X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
```

Save the file as `sample.txt`, attach it to an email, and then send the email to a verified address\. If there are no other issues with the email, Amazon SES accepts it, but then rejects it as it would if it contained an actual virus\.

**Note**  
Rejected emails—including those that you send by using the procedure above—are counted against your daily sending quota\. You are billed for each message you send, including those that are rejected\.

To learn more about the EICAR test file, see the [EICAR test file page on Wikipedia](https://en.wikipedia.org/wiki/EICAR_test_file)\. For code examples that you can use to send messages with attachments, see [Sending Raw Email using AWS SDKs](examples-send-raw-using-sdk.md)\.


****  

|  | 
| --- |
| For technical discussions about various Amazon SES topics, visit the [Amazon SES Blog](https://aws.amazon.com//blogs/ses/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 