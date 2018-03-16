# Amazon SES IP Blacklist FAQs<a name="blacklists"></a>

Mailbox providers try to protect their users from receiving spam\. To determine whether an incoming email is spam, mailbox providers examine various characteristics of the email\. One of the methods they use is to check whether the IP address that sent the email is on an *IP blacklist*\. An IP blacklist, also known as *realtime blackhole list \(RBL\)* or *DNS\-based blackhole list \(DNSBL\)*, is a list of IP addresses suspected to send spam\. If the IP address that the email was sent from is on a blacklist that a mailbox provider uses, the mailbox provider might bounce the email or drop it entirely\.

As with IP addresses of other email service providers, Amazon SES IP addresses occasionally appear on blacklists\. This topic describes how blacklists impact your ability to send email using Amazon SES\.

**Note**  
This topic is about blacklists that mailbox providers use to block incoming mail from email service providers such as Amazon SES\. If you are looking for information about how Amazon SES blocks *outgoing* mail to recipient addresses that have previously bounced, see [Removing an Email Address from the Amazon SES Suppression List](remove-from-suppression-list.md)\.

## Q1\. How do I know if blacklisting is preventing recipients from receiving my emails?<a name="bl-q1"></a>

If your sending is impacted by the blacklisting of an Amazon SES IP address, you'll probably receive a [bounce notification](monitor-sending-using-notifications.md) that contains a message indicating that your email was rejected because of a listing on a blacklist\. In most cases, the bounce message includes the name or URL of the blacklist\. An example of this type of message is "Message rejected due to IP \[0\.0\.0\.0\] listed on RBL \[X\]"\. If you receive a bounce notification that doesn't contain this type of information, it's unlikely that the email bounced as a result of the sending address being blacklisted\.

## Q2\. What does Amazon SES do about blacklisting?<a name="bl-q2"></a>

There are two types of IP addresses that you can use when sending email through Amazon SES: dedicated IP addresses, which are reserved for your exclusive use, and shared IP addresses, which are shared by all Amazon SES users\.

If you use dedicated IP addresses, you're responsible for maintaining the reputations of those addresses\. However, the Amazon SES team maintains the reputations of the shared IP addresses\. We actively monitor the messages that are sent through Amazon SES to remove senders who violate the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. We also work closely with major mailbox providers and blacklist operators to resolve blacklisting events\. Because each blacklist operator has its own de\-listing policies, we can't provide estimates for the amount of time it will take to remove an address from a blacklist\.

Occasionally, one of our IP addresses is listed on the SORBS, UCEPROTECT, or SpamCannibal lists\. Our data indicates that the impact of these blacklists is extremely low\. We aren't aware of any major mailbox providers using these blacklists to reject messages\. Resolving these listings would require a large amount of effort, but wouldn't provide any benefit for Amazon SES customers\. For this reason, we don't actively work with these vendors to resolve blacklisting events\.

## Q3\. Are all blacklists equally important?<a name="bl-q3"></a>

Not all blacklist providers are equal in their importance\. Major mailbox providers use commercial blacklist products such as Spamhaus, which are designed not only to blacklist actual spammers, but also to minimize the number of legitimate addresses that are blacklisted\. A Spamhaus listing is likely to result in email being blocked at major mailbox providers, while a listing on a free blacklist service probably won't have much impact on the delivery of your emails\.