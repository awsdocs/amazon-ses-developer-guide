# Using Dedicated IP Addresses with Amazon SES<a name="dedicated-ips"></a>

When you create a new Amazon SES account, your emails are sent from IP addresses that are shared with other Amazon SES users\. For [an additional monthly charge](https://aws.amazon.com/ses/pricing), you can lease dedicated IP addresses that are reserved for your exclusive use\. Both of these options offer unique benefits and drawbacks, which are summarized in the following table; click an item in the **Benefit** column for additional information about that benefit\.


****  

| Benefit | Shared IP addresses | Dedicated IP addresses | 
| --- | --- | --- | 
| [Ready to use with no additional setup](#dedicated-ips-simplicity) | Yes | No | 
| [Reputation managed by AWS](#dedicated-ips-managed-reputation) | Yes | No | 
| [Good for customers with continuous, predictable sending patterns](#dedicated-ips-sending-patterns) | Yes | Yes | 
| [Good for customers with less predictable sending patterns](#dedicated-ips-sending-patterns) | Yes | No | 
| [Good for high\-volume senders](#dedicated-ips-sending-volumes) | Yes | Yes | 
| [Good for low\-volume senders](#dedicated-ips-sending-volumes) | Yes | No | 
| [Additional monthly costs](#dedicated-ips-costs) | No | Yes | 
| [Complete control over sender reputation](#dedicated-ips-reputation-control) | No | Yes | 
| [Isolate reputation by email type, recipient, or other factors](#dedicated-ips-isolate-reputation) | No | Yes | 
| [Provides known IP addresses that never change](#dedicated-ips-known-addresses) | No | Yes | 

**Important**  
If you do not plan to send large volumes of email on a regular and predictable basis, we recommend using shared IP addresses\. If you use dedicated IP addresses in situations where you are sending low volumes of mail, or if your sending patterns are highly irregular, you will experience deliverability issues\.

## Ease of Setup<a name="dedicated-ips-simplicity"></a>

If you choose to use shared IP addresses, then you do not need to perform any additional configuration; your Amazon SES account is ready to send email as soon as you verify an email address and move out of the sandbox\.

If you choose to lease dedicated IP addresses, you must determine how many dedicated IP addresses you need, submit a request, and optionally set up dedicated IP pools\.

## Reputation Managed by AWS<a name="dedicated-ips-managed-reputation"></a>

IP address reputations are based largely on historical sending patterns and volume\. An IP address that sends consistent volumes of email over a long period of time usually has a good reputation\.

Shared IP addresses are used by several Amazon SES customers\. Together, these customers send a large volume of email\. AWS carefully manages this outbound traffic in order to maximize the reputations of the shared IP addresses\.

If you use dedicated IP addresses, it is your responsibility to maintain your sender reputation by sending consistent and predictable volumes of email\.

## Predictability of Sending Patterns<a name="dedicated-ips-sending-patterns"></a>

An IP address with a consistent history of sending email has a better reputation than one that suddenly starts sending out large volumes of email with no prior sending history\.

If your email sending patterns are irregular—that is, they do not follow a predictable pattern—then shared IP addresses are probably a better fit your needs\. When you use shared IP addresses, you can increase or decrease your email sending patterns as the situation demands\.

If you use dedicated IP addresses, you must warm up those addresses by sending an amount of email that gradually increases every day\. The process of warming up new IP addresses is described in [Warming up Dedicated IP Addresses](dedicated-ip-warming.md)\. Once your dedicated IP addresses are warmed up, you must then maintain a consistent sending pattern\.

## Volume of Outbound Email<a name="dedicated-ips-sending-volumes"></a>

Dedicated IP addresses are best suited for customers who send large volumes of email\. Most internet service providers \(ISPs\) only track the reputation of a given IP address if they receive a significant volume of mail from that address\. For each ISP with which you want to cultivate a reputation, you should send several hundred emails within a 24\-hour period at least once per month\.

In some cases, you may be able to use dedicated IP addresses if you do not send large volumes of email\. For example, dedicated IP addresses may work well if you send to a small, well\-defined group of recipients whose mail servers accept or reject email using a list of specific IP addresses, rather than IP address reputation\. 

## Additional Costs<a name="dedicated-ips-costs"></a>

The use of shared IP addresses is included in the standard Amazon SES pricing\. Leasing dedicated IP addresses incurs an extra monthly cost beyond the standard costs associated with sending email using Amazon SES\. Each dedicated IP address incurs a separate monthly charge\. For pricing information, see the [Amazon SES pricing page](https://aws.amazon.com/ses/pricing/)\.

## Control over Sender Reputation<a name="dedicated-ips-reputation-control"></a>

When you use dedicated IP addresses, your Amazon SES account is the only one that is able to send email from those addresses\. For this reason, the sender reputation of the dedicated IP addresses that you lease is determined by your email sending practices\.

## Ability to Isolate Sender Reputation<a name="dedicated-ips-isolate-reputation"></a>

By using dedicated IP addresses, you can isolate your sender reputation for different components of your email program\. If you lease more than one dedicated IP address for use with Amazon SES, you can create *dedicated IP pools*—groups of dedicated IP addresses that can be used for sending specific types of email\. For example, you can create one pool of dedicated IP addresses for sending marketing email, and another for sending transactional email\. To learn more, see [Creating Dedicated IP Pools](dedicated-ip-pools.md)\.

## Known, Unchanging IP Addresses<a name="dedicated-ips-known-addresses"></a>

When you use dedicated IP addresses, you can find the values of the addresses that send your mail in the **Dedicated IPs** page of the Amazon SES console\. Dedicated IP addresses do not change\. 

With shared IP addresses, you do not know the IP addresses that Amazon SES uses to send your mail, and they can change at any time\.