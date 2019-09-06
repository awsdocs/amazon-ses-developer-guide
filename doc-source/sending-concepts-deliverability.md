# Amazon SES and Deliverability<a name="sending-concepts-deliverability"></a>

You want your recipients to read your emails, find them valuable, and not label them as spam\. In other words, you want to maximize email *deliverability*—the percentage of your emails that arrive in your recipients' inboxes\. This topic reviews email deliverability concepts that you should be familiar with when you use Amazon SES\.

To maximize email deliverability, you need to understand email delivery issues, proactively take steps to prevent them, stay informed of the status of the emails that you send, and then improve your email\-sending program, if necessary, to further increase the likelihood of successful deliveries\. The following sections review the concepts behind these steps and how Amazon SES helps you through the process\. 

![\[\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/deliverability_concepts-diagram.png)

## Understand Email Delivery Issues<a name="sending-concepts-deliverability-understanding"></a>

In most cases, your messages are delivered successfully to recipients who expect them\. In some cases, however, a delivery might fail, or a recipient might not want to receive the mail that you are sending\. Bounces, complaints, and the suppression list are related to these delivery issues and are described in the following sections\. 

### Bounce<a name="sending-concepts-deliverability-bounce"></a>

If your recipient's receiver \(for example, an ISP\) fails to deliver your message to the recipient, the receiver bounces the message back to Amazon SES\. Amazon SES then notifies you of the bounced email through email or through Amazon Simple Notification Service \(Amazon SNS\), depending on how you have your system set up\. For more information, see [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\.

There are *hard bounces* and *soft bounces*, as follows: 
+ **Hard bounce – **A persistent email delivery failure\. For example, the mailbox does not exist\. Amazon SES does not retry hard bounces, with the exception of DNS lookup failures\. We strongly recommend that you do not make repeated delivery attempts to email addresses that hard bounce\.
+ **Soft bounce – **A temporary email delivery failure\. For example, the mailbox is full, there are too many connections \(also called *throttling*\), or the connection times out\. Amazon SES retries soft bounces multiple times\. If the email still cannot be delivered, then Amazon SES stops retrying it\.

Amazon SES notifies you of hard bounces and soft bounces that will no longer be retried\. However, only hard bounces count toward your bounce rate and the bounce metric that you retrieve using the Amazon SES console or the `GetSendStatistics` API\.

Bounces can also be *synchronous* or *asynchronous*\. A synchronous bounce occurs while the email servers of the sender and receiver are actively communicating\. An asynchronous bounce occurs when a receiver initially accepts an email message for delivery and then subsequently fails to deliver it to the recipient\.

### Complaint<a name="sending-concepts-deliverability-complaint"></a>

Most email client programs provide a button labeled "Mark as Spam," or similar, which moves the message to a spam folder, and forwards it to the ISP\. Additionally, most ISPs maintain an abuse address \(e\.g\., abuse@example\.net\), where users can forward unwanted email messages and request that the ISP take action to prevent them\. In both of these cases, the recipient is making a complaint\. If the ISP concludes that you are a spammer, and Amazon SES has a feedback loop set up with the ISP, then the ISP will send the complaint back to Amazon SES\. When Amazon SES receives such a complaint, it forwards the complaint to you either by email or by using an Amazon SNS notification, depending on how you have your system set up\. For more information, see [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\. We recommend that you do not make repeated delivery attempts to email addresses that generate complaints\. 

### Suppression List<a name="sending-concepts-deliverability-suppression-list"></a>

The Amazon SES *suppression list* is a list of recipient email addresses that have recently caused a hard bounce for any Amazon SES customer\. If you try to send an email through Amazon SES to an address that is on the suppression list, the call to Amazon SES succeeds, but Amazon SES treats the email as a hard bounce instead of attempting to send it\. Like any hard bounce, suppression list bounces count towards your sending quota and your bounce rate\. An email address can remain on the suppression list for up to 14 days\. If you are sure that the email address that you're trying to send to is valid, you can submit a suppression list removal request\. For more information, see [Removing an Email Address from the Amazon SES Suppression List](remove-from-suppression-list.md)\. 

## Be Proactive<a name="sending-concepts-deliverability-be-proactive"></a>

One of the biggest issues with email on the Internet is unsolicited bulk email, or spam\. ISPs take considerable measures to prevent their customers from receiving spam\. Correspondingly, Amazon SES takes proactive steps to decrease the likelihood that ISPs consider your email to be spam\. Amazon SES uses verification, authentication, sending limits, and content filtering\. Amazon SES also maintains a trusted reputation with ISPs and requires you to send high\-quality email\. Amazon SES does some of those things for you automatically \(like content filtering\); in other cases, it provides the tools \(like authentication\), or guides you in the right direction \(sending limits\)\. The following sections provide more information about each concept\.

### Verification<a name="sending-concepts-deliverability-verification"></a>

Unfortunately, it's possible for a spammer to falsify an email header and spoof the originating email address so that it appears as though the email originated from a different source\. To maintain trust between ISPs and Amazon SES, Amazon SES needs to ensure that its senders are who they say they are\. You are therefore required to verify all email addresses from which you send emails through Amazon SES to protect your sending identity\. You can verify email addresses by using the Amazon SES console or by using the Amazon SES API\. You can also verify entire domains\. For more information, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md) and [Verifying Domains in Amazon SES](verify-domains.md)\.

If your account is still in the Amazon SES sandbox, you also need to verify all recipient addresses except for addresses provided by the Amazon SES mailbox simulator\. For information about getting out of the sandbox, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\. For more information about the mailbox simulator, see [Testing Email Sending in Amazon SES](mailbox-simulator.md)\.

### Authentication<a name="sending-concepts-deliverability-authentication"></a>

*Authentication* is another way that you can indicate to ISPs that you are who you say you are\. When you authenticate an email, you provide evidence that you are the owner of the account and that your emails have not been modified in transit\. In some cases, ISPs refuse to forward email that is not authenticated\. Amazon SES supports two methods of authentication: Sender Policy Framework \(SPF\) and DomainKeys Identified Mail \(DKIM\)\. For more information, see [Authenticating Your Email in Amazon SES](authentication.md)\.

### Sending Limits<a name="sending-concepts-deliverability-sending-limits"></a>

If an ISP detects sudden, unexpected spikes in the volume or rate of your emails, the ISP might suspect you are a spammer and block your emails\. Therefore, every Amazon SES account has a set of sending limits to regulate the number of email messages that you can send and the rate at which you can send them\. These sending limits help you to gradually ramp up your sending activity to protect your trustworthiness with ISPs\.

Amazon SES has two sending limits: a sending quota \(the maximum number of messages you can send in a 24\-hour period\) and a maximum send rate \(the maximum number of emails that Amazon SES can accept from your account per second, although the actual rate at which Amazon SES accepts your messages might be less than the maximum send rate\)\. If you are a brand\-new user, Amazon SES lets you send a small amount of email each day\. If the mail that you send is acceptable to ISPs, this limit will gradually increase\. Over time, your sending limits will steadily increase so that you can send larger quantities of email at faster rates\. You can also file an [SES Sending Limits Increase case](https://aws.amazon.com/ses/extendedaccessrequest/) to get your quotas increased if you need them to ramp up more quickly\.

For more information about sending limits and how to increase them, see [Managing Your Amazon SES Sending Limits](manage-sending-limits.md)\.

### Content Filtering<a name="sending-concepts-deliverability-content-filtering"></a>

Many ISPs use content filtering to determine if incoming emails are spam\. Content filters look for questionable content and block the email if the email fits the profile of spam\. Amazon SES uses content filters also\. When your application sends a request to Amazon SES, Amazon SES assembles an email message on your behalf and then scans the message header and body to determine if they contain content that ISPs might construe as spam\. If your messages look like spam to the content filters that Amazon SES uses, your reputation with Amazon SES will be negatively affected\. 

Amazon SES also scans all messages for viruses\. If a message contains a virus, Amazon SES doesn't attempt to deliver the message to the recipient's mail server\.

### Reputation<a name="sending-concepts-deliverability-reputation"></a>

When it comes to email sending, *reputation*—a measure of confidence that an IP address, email address, or sending domain is not the source of spam—is important\. Amazon SES maintains a strong reputation with ISPs so that ISPs deliver your emails to your recipients' inboxes\. Similarly, you need to maintain a trusted reputation with Amazon SES\. You build your reputation with Amazon SES by sending high\-quality content\. When you send high\-quality content, your reputation becomes more trusted over time and Amazon SES increases your sending limits\. Excessive bounces and complaints negatively impact your reputation and can cause Amazon SES to lower your sending limits or terminate your Amazon SES account\.

One way to help maintain your reputation is to use the mailbox simulator when you test your system, instead of sending to email addresses that you have created yourself\. Emails to the mailbox simulator do not count toward your bounce and complaint metrics\. For more information about the mailbox simulator, see [Testing Email Sending in Amazon SES](mailbox-simulator.md)\.

### High\-Quality Email<a name="sending-concepts-deliverability-high-quality-email"></a>

High\-quality email is email that recipients find valuable and want to receive\. Value means different things to different recipients and can come in the form of offers, order confirmations, receipts, newsletters, etc\. Ultimately, your deliverability rests on the quality of the emails that you send because ISPs block emails that they find to be low quality \(spam\)\. 

## Stay Informed<a name="sending-concepts-deliverability-stay-informed"></a>

Whether your deliveries fail, your recipients complain about your emails, or Amazon SES successfully delivers an email to a recipient's mail server, Amazon SES helps you to track down the issue by providing notifications and by enabling you to easily monitor your usage statistics\.

### Notifications<a name="sending-concepts-deliverability-feedback-notifications"></a>

When an email bounces, the ISP notifies Amazon SES, and Amazon SES notifies you\. Amazon SES notifies you of hard bounces and soft bounces that Amazon SES will no longer retry\. Many ISPs also forward complaints, and Amazon SES sets up complaint feedback loops with the major ISPs so you don't have to\. Amazon SES can notify you of bounces, complaints, and successful deliveries in two ways: you can set your account up to receive notifications through Amazon SNS, or you can receive notifications by email \(bounces and complaints only\)\. For more information, see [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\.

### Usage Statistics<a name="sending-concepts-deliverability-usage-statistics"></a>

Amazon SES provides usage statistics so that you can view your failed deliveries to determine and resolve the root causes\. You can view your usage statistics by using the Amazon SES console or by calling the Amazon SES API\. You can view how many deliveries, bounces, complaints, and virus\-infected rejected emails you have, and you can also view your sending limits to ensure that you stay within them\.

## Improve Your Email\-Sending Program<a name="sending-concepts-deliverability-improve"></a>

If you are getting large numbers of bounces and complaints, it's time to reassess your email\-sending strategy\. Remember that excessive bounces, complaints, and attempts to send low\-quality email constitute abuse and put your AWS account at risk of termination\. Ultimately, you need to be sure that you use Amazon SES to send high\-quality emails and to only send emails to recipients who want to receive them\.  