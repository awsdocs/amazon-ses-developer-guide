# Amazon SES IP Blacklist FAQs<a name="blacklists"></a>

Mailbox providers aim to protect their users from receiving spam\. To determine whether an incoming email is spam, mailbox providers examine various characteristics of the email\. One of the methods they use is to check whether the IP address that sent the email is on an *IP blacklist*\. An IP blacklist, also known as *realtime blackhole list \(RBL\)* or *DNS\-based blackhole list \(DNSBL\)*, is a list of IP addresses suspected to send spam\. If the IP address from which the email was sent is on a blacklist that a mailbox provider uses, the mailbox provider might bounce the email or drop it entirely\. Various organizations compile blacklists\. Mailbox providers choose which of these blacklists they use\. Some blacklists are more widely used than others\.

As with IP addresses of other email service providers, Amazon SES IP addresses occasionally appear on blacklists\. This topic describes how blacklists impact your sending with Amazon SES\.

**Note**  
This topic is about blacklists that mailbox providers use to block incoming mail from email service providers such as Amazon SES\. If you are looking for information about how Amazon SES blocks *outgoing* mail to recipient addresses that have previously bounced, see [Removing an Email Address from the Amazon SES Suppression List](remove-from-suppression-list.md)\.

## Q1\. How do I know if blacklisting is preventing recipients from receiving my emails?<a name="bl-q1"></a>

If your sending is impacted by the blacklisting of an Amazon SES IP address, you will most likely receive a bounce notification that contains a message indicating that your email was rejected because of a listing on a blacklist\. In most cases, the bounce message includes the name or URL of the blacklist\. An example of this type of message is "Message rejected due to IP \[0\.0\.0\.0\] listed on RBL \[X\]"\. If you do not see this type of message in your bounce notifications, it is unlikely that a blacklist is impacting your sending\.

## Q2\. What does Amazon SES do about blacklisting?<a name="bl-q2"></a>

We carefully and continually monitor the reputation of our IP address space\. We work closely with mailbox providers and blacklist operators to identify and mitigate blacklistings in parallel with our routine compliance processes to remove offending users from our system\.

Blacklist operators set their own listing and delisting policies, which do not always comply with the recommendations in [RFC 6471](https://tools.ietf.org/html/rfc6471#section-2), so we cannot provide a guarantee or timeframe in which a blacklisting will be resolved\. In particular, we are aware that portions of our IP address space are sometimes listed on [SORBS](http://www.sorbs.net/) and [UCEPROTECT](http://www.uceprotect.net/en/index.php)\. We frequently work with blacklist data to mitigate and resolve the listings, but we cannot estimate the time to resolution\.

There are some blacklists, such as SORBS and SpamCannibal, that we are not currently working with to resolve listings\. Our data indicates that the impact of these blacklists is extremely low\. We are not aware of any major mailbox provider using SORBS or SpamCannibal to reject messages\. If you are particularly concerned about SORBS, we recommend doing an internet search of "SORBS reliability" to get an idea of why ISPs should never use this blacklist as the sole criteria for rejecting mail\.

## Q3\. Are all blacklists equally important?<a name="bl-q3"></a>

No\. Large mailbox providers typically use reputable blacklists such as [Spamhaus](http://spamhaus.org/), whereas smaller mailbox providers who aren't aware of which blacklists are reputable might use less reputable blacklists\. A Spamhaus listing is likely to result in mail being blocked at major mailbox providers, while a listing on SORBS, UCEPROTECT, or SpamCannibal might not have much impact on your sending\. A service that asks you to pay for delisting or does not accept delist requests is unlikely to be used by major mailbox providers\.

## Q4\. What should I do if I notice that an Amazon SES IP address is on a blacklist?<a name="bl-q4"></a>

Rest assured that we are aware of the listing, and are working towards a resolution\. However, remember that if you do not receive bounce notifications that contain a message about blacklisting, it is very unlikely that a blacklisting is impacting your sending\. If your bounce notifications do indicate that a SORBS, UCEPROTECT, or SpamCannibal listing is impacting your sending, we recommend that you contact the mailbox provider and make sure that it is not using any of these as the sole criteria for blocking IP addresses\.

## Q5\. Emails I send are being put in the junk folder\. Could this be because an Amazon SES IP address is on a blacklist?<a name="bl-q5"></a>

It is unlikely that your sending is impacted by blacklisting unless your bounce notifications contain a message indicating that blacklisting is the reason for the rejection of the message\. 