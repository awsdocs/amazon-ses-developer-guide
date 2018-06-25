# Amazon SES IP Blacklist FAQs<a name="blacklists"></a>

IP blacklists are lists of IP addresses that the maintainer of the list suspects of sending unsolicited content\. Blacklists are sometimes called *Realtime Blackhole Lists* \(RBLs\) or *DNS\-based Blackhole Lists* \(DNSBLs\)\. Blacklists are meant to be used as one of several factors that email providers consider when they determine whether an incoming email is spam\. They are never meant to be the *only* factor used to make this decision\.

As with the IP addresses that belong to other email service providers, the IP addresses associated with Amazon SES might occasionally appear on some blacklists\. This topic describes how blacklists might impact the delivery of emails you send using Amazon SES, as well as our policies for removing our IP addresses from blacklists\.

**Note**  
This topic is about the blacklists that email providers use to block incoming messages\. For information about how Amazon SES blocks outgoing email sent to recipients whose email addresses have previously generated bounces, see [Removing an Email Address from the Amazon SES Suppression List](remove-from-suppression-list.md)\.

## Q1\. How do blacklists impact email delivery?<a name="bl-q1"></a>

There are thousands of email providers around the world, and there are thousands of blacklists available on the internet\. For this reason, there isn't an answer to this question that applies to all email providers and all blacklists\.

Our research shows that major email providers—including Gmail, Hotmail, and Yahoo—rely on a very small number of highly regarded blacklists, such as those offered by Spamhaus\. Smaller blacklists have a much lower impact on delivery among larger email providers\.

Finally, major email providers often have their own internal blacklists\. Email providers usually guard these lists very closely, and rarely share them with the public\. If an IP address is on one of these lists, it can have a major impact on deliverability for that provider\.

## Q2\. How do IP addresses end up on blacklists?<a name="bl-q2"></a>

There are several ways that an IP address can end up on a blacklist\. Most often, IP addresses are added to blacklists when they send email to a *spamtrap*\. A spamtrap is an email address that doesn't belong to a human user, but rather exists solely to collect spam and identify spammers\. Some blacklists also allow individual users to submit IP addresses\. A few blacklists even allow users to submit entire IP address ranges\. This is an especially bad practice, because it can result in a very large number of false positive results\.

## Q3\. How does Amazon SES prevent its IP addresses from appearing on blacklists?<a name="bl-q3"></a>

We monitor the messages that users send through Amazon SES\. If a sender's email results in a large number of bounces or complaints, it usually indicates that the sender has a larger issue that might result in the IP addresses they use being added to a blacklist\. When these situations arise, we might pause that sender's ability to send more email until they resolve the issue that caused the bounces or complaints in the first place\.

Additionally, if a sender's email contains content that's typical of spam, or content that leads us to believe that the email is unsolicited, we might also pause that sender's ability to send email\.

## Q4\. Can Amazon SES have its IP addresses removed from a blacklist?<a name="bl-q4"></a>

We actively monitor major blacklist providers, with a special focus on Spamhaus because of its enormous impact on deliverability among major email providers\. When one of our IP addresses is listed by Spamhaus or another major blacklist provider, we work directly with that provider to have the address removed from the list as quickly as possible\. 

For other blacklist providers, we review each listing on a case\-by\-case basis\. If we determine that the presence of an Amazon SES IP address on a blacklist is the cause of delivery issues, we contact the provider to request the removal of the IP address\. However, different blacklists have different rules for listing and removing \("de\-listing"\) IP addresses\. Some blacklists won't de\-list IP addresses until a certain period of time has elapsed without receiving additional spam reports\. For addresses that have been listed multiple times, this period can be one year or more\. We can't guarantee that we'll be able to remove IP addresses from these blacklists in a timely manner\. Furthermore, because a large number of senders use Amazon SES, it's possible for IP addresses to be added back to these blacklists shortly after being removed\. 

## Q5\. An email provider is rejecting my email because the sending IP address is listed by a blacklist other than Spamhaus\. What can I do?<a name="bl-q5"></a>

First, confirm that the message was truly blocked because of a blacklist\. If your email was rejected because the sending IP address was blacklisted, you typically receive a bounce notification that contains a message similar to the following example:

```
554 5.7.1 Service unavailable; Client host [192.0.2.0] blocked using blacklistName; See: http://www.example.com/query/ip/192.0.2.0
```

If you receive a bounce notification, but it doesn't contain information similar to the message shown in the preceding example, then the email provider most likely rejected your message for a reason unrelated to blacklisting\.

Next, consider which blacklist the IP address is listed on\. In general, large email providers—such as Gmail, Hotmail, and Yahoo—only consider highly reputable blacklists \(such as Spamhaus\) when they determine which messages to accept and which to reject\. The presence of your sending IP address on a blacklist other than Spamhaus typically doesn't have any impact on your ability to send email to customers who use major email providers\.

Other email providers might not have the same amount of expertise in setting up spam filtering systems as their larger counterparts\. Some email providers, especially smaller ones, rely on less reputable blacklists to reject email\. Rejecting email solely on the basis of a blacklisting—regardless of the size or reputation of the list—is considered a bad practice, because it can cause a large amount of legitimate email to be blocked\. Smaller blacklists are more likely to list legitimate IP addresses than lists offered by companies such as Spamhaus\.

If an email provider is blocking your email because your sending IP address is listed on a blacklist other than Spamhaus, there are a few things you can do\. First, contact the postmaster of the domain that rejected your message\. Demonstrate to the postmaster that you're only sending legitimate email that customers explicitly asked to receive\. Encourage them to consider a less\-restrictive spam filtering policy, or to make an exception for your messages\.

If the email provider is unable or unwilling to change its policies, consider using a dedicated IP address\. Dedicated IP addresses are IP addresses that only you can use\. By adopting good sending practices, you can keep your engagement rates high, and your rates of bounces, complaints, and spamtrap hits very low\. These practices can help ensure that your IP addresses aren't added to blacklists\.

## Q6\. Email that I send to Gmail, Yahoo, Hotmail, or another major provider is being sent to the spam folder\. Is this happening because my sending IP address is on a blacklist?<a name="bl-q6"></a>

Major email providers place much more emphasis on recent engagement than on blacklists\. Recent engagement refers to the number of customers who have read or clicked links in messages that you sent within the past 30–90 days\. If your sending address isn't listed by Spamhaus, then it's extremely unlikely the inbox placement issue you're experiencing is related to your sending address being blacklisted\.

To increase the chances that your messages reach your customers' inboxes, you should implement all of the following best practices:
+ **Never** rent or purchase lists of email addresses\.
+ Only send email to customers who explicitly asked to receive email from you\.
+ Use consistent design elements and writing styles in each message that you send to ensure that customers can easily identify messages from you\.
+ When customers use a web form to subscribe to your content, send them an email to confirm that they want to receive email from you\. Don't send them any additional email until they confirm that they want to receive email from you\. This process is known as *double opt\-in*\.
+ Make it easy for your customers to unsubscribe, and honor unsubscribe requests immediately\.
+ Stop sending email to customers who haven't opened or clicked links in messages that you've sent in the past 30–90 days\.
+ If you send email that contains links, check those links against the Spamhaus Domain Block List \(DBL\)\. To test your links, use the [Domain Lookup Tool](https://www.spamhaus.org/lookup/) on the Spamhaus website\.

Implementing these practices can help keep your rates of bounces and complaints low, and can lower the risk of sending email to spamtraps\. By reducing the number of bounces, complaints, and spamtrap hits, you can improve your sender reputation, and can make it more likely that the email you send arrives in your customers' inboxes\.