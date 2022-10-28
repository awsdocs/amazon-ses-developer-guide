# Dedicated IP addresses for Amazon SES<a name="dedicated-ip"></a>

When you create a new Amazon SES account, by default your emails are sent from IP addresses that are shared with other SES users\.  You can also use dedicated IP addresses that are reserved for your exclusive use by leasing them for [an additional cost](https://aws.amazon.com/ses/pricing)\. This gives you complete control over your sender reputation and enables you to isolate your reputation for different segments within email programs\. Amazon SES offers two ways to provision and manage a dedicated IP address:
+ **Standard**—refers to dedicated IP addresses that you manually set up and manage, including the option to manually warm them up and scale them out, and to manually move them in and out of IP pools\. \(These were formally referred to as *dedicated IP addresses* in SES\.\) 
+ **Managed**—refers to dedicated IP addresses that are automatically set up on your behalf by SES to provide a quick and easy way to start using dedicated IP addresses that are managed by SES; they automatically warm up for each ISP individually and auto\-scale based on your sending volume to help ensure that your dedicated IP addresses are used optimally based on how you send email\.

When deciding between shared IP addresses or the dedicated IP addresses defined above, choose the one that provides the most benefits for the type, volume, and patterns of email that you send\. To help you make your decision, these benefits are summarized in the following table\. Choose an item in the **Benefit** column for additional information\.


****  

| Benefit | Shared IP addresses | Dedicated IP addresses \(standard\) | Dedicated IP addresses \(managed\) | 
| --- | --- | --- | --- | 
| [Ready to use immediately](#dedicated-ip-simplicity) | Yes | No | Yes | 
| [Additional setup required](#dedicated-ip-simplicity) | No | Yes | Yes | 
| [Reputation managed by AWS](#dedicated-ip-managed-reputation) | Yes | No | No | 
| [Good for customers with continuous, predictable sending patterns](#dedicated-ip-sending-patterns) | Yes | Yes | Yes | 
| [Good for customers with less predictable sending patterns](#dedicated-ip-sending-patterns) | Yes | No | Yes | 
| [Good for high\-volume senders](#dedicated-ip-sending-volumes) | Yes | Yes | Yes | 
| [Good for low\-volume senders](#dedicated-ip-sending-volumes) | Yes | No | No | 
| [Additional monthly costs](#dedicated-ip-costs) | No | Yes | Yes | 
| [Complete control over sender reputation](#dedicated-ip-reputation-control) | No | Yes | Yes | 
| [Isolate reputation by email type, recipient, or other factors](#dedicated-ip-isolate-reputation) | No | Yes | Yes | 
| [Provides known IP addresses that never change](#dedicated-ip-known-addresses) | No | Yes | No | 

**Important**  
If you don't plan to send large volumes of email on a regular and predictable basis, we recommend that you use shared IP addresses\. If you use dedicated IP addresses in situations where you're sending low volumes of mail, or if your sending patterns are highly irregular, you might experience deliverability issues\. In this scenario, using *Dedicated IPs \(managed\)* is the better option\.

## Ease of setup<a name="dedicated-ip-simplicity"></a>

**Shared IP addresses**—you don't need to perform any additional configuration\. Your SES account is ready to send email as soon as you verify an email address and move out of the sandbox\.

**Dedicated IP addresses \(standard\)**—you must [submit a request](dedicated-ip-case.md) through the AWS Support Center and optionally [configure dedicated IP pools](dedicated-ip-pools.md)\.

**Dedicated IP addresses \(managed\)**—you don’t need to submit a request for dedicated IP addresses\. They'll automatically be allocated when you opt in and do a one\-time walkthrough to create your managed dedicated pool\.

## Reputation management<a name="dedicated-ip-managed-reputation"></a>

IP address reputations are based largely on historical sending patterns and volume\. An IP address that sends consistent volumes of email over a long period of time typically has a good reputation\.

**Shared IP addresses**—shared between several SES customers, these addresses collectively send a large volume of email and AWS carefully manages the outbound traffic to maximize the reputations of the shared IP addresses\.

**Dedicated IP addresses \(standard\)**—you maintain your sender reputation by sending consistent and predictable volumes of email\.

**Dedicated IP addresses \(managed\)**—adds the benefit of tracking the reputation for each ISP and optimally scheduling outgoing sending accordingly\. So while you still maintain your sender reputation, this automation helps to improve overall deliverability and reduce bounce rates when compared to equivalent workloads on manually configured dedicated IP addresses\. 

**Note**  
For information about Smart Network Data Services \(SNDS\) data for your dedicated IPs, see [SNDS metrics for dedicated IPs](snds-metrics-dedicated-ips.md)\.

## Predictability of sending patterns<a name="dedicated-ip-sending-patterns"></a>

An IP address with a consistent history of sending email has a better reputation than one that suddenly starts sending out large volumes of email with no prior sending history\.

**Shared IP addresses**—good for email sending patterns that don't follow a predictable pattern\. With shared IP addresses, you can increase or decrease your email sending patterns as the situation demands\.

**Dedicated IP addresses \(standard\)**—you must warm up addresses by sending an amount of email that gradually increases every day\. The process of warming up new IP addresses is described in [Warming up dedicated IP addresses \(standard\)](dedicated-ip-warming.md)\. After your dedicated IP addresses are warmed up, you must then maintain a consistent sending pattern\.

**Dedicated IP addresses \(managed\)**—your dedicated IP addresses are warmed up automatically for each IP in the managed pool by using an adaptive warmup strategy that takes into account actual sending patterns to optimize the warmup for each ISP individually\. 

## Volume of outbound email<a name="dedicated-ip-sending-volumes"></a>

**Shared IP addresses** —best for customers who send low volumes of email\.

**Dedicated IP addresses \(standard\) \| Dedicated IP addresses \(managed\)**—both are suited for customers who send large volumes of email\. Most ISPs only track the reputation of a given IP address if they receive a significant volume of mail from that address\. For each ISP with which you want to cultivate a reputation, you should send several hundred emails within a 24\-hour period at least once per month\. In some cases, both types of dedicated IP addresses may also work for smaller volumes of email\. For example, they may work well if you send to a small, well\-defined group of recipients whose mail servers accept or reject email using a list of specific IP addresses, rather than IP address reputation\.

## Additional costs<a name="dedicated-ip-costs"></a>

**Shared IP addresses**—included in the standard SES pricing\.

**Dedicated IP addresses \(standard\)**—are available for an additional monthly fee per IP address that you lease\. For pricing information, see the [SES pricing page](https://aws.amazon.com/ses/pricing/)\.

**Dedicated IP addresses \(managed\)**—are available for a standard monthly fee \(regardless of the amount of IPs needed\) and a per message usage charge\. For pricing information, see the [SES pricing page](https://aws.amazon.com/ses/pricing/)\.

## Control over sender reputation<a name="dedicated-ip-reputation-control"></a>

**Shared IP addresses**—your sender reputation is controlled by SES\.

**Dedicated IP addresses \(standard\) \| Dedicated IP addresses \(managed\)**—your sender reputation is completely under your control\. Your SES account is the only one that is able to send email from those addresses\. For this reason, the sender reputation is determined by your email sending practices\.  Additionally, dedicated IPs \(managed\) actively monitors outbound IP addresses used for email sending by using the highest performing IP addresses to improve email deliverability to your recipients\. Utilization data can be surfaced by using additional services such as Amazon CloudWatch metrics and the built\-in dashboards that are in Amazon SES\. 

## Ability to isolate sender reputation<a name="dedicated-ip-isolate-reputation"></a>

**Shared IP addresses**—your sender reputation is set at the account level and can't be isolated\.

**Dedicated IP addresses \(standard\) \| Dedicated IP addresses \(managed\)**—you can isolate your sender reputation for different components within your email program  by creating *dedicated IP pools*—groups of dedicated IP addresses that can be used for sending specific types of email\. For example, you can create one pool of dedicated IP addresses for sending marketing email, and another for sending transactional email\. 

## Known, unchanging IP addresses<a name="dedicated-ip-known-addresses"></a>

**Shared IP addresses**—you don't know the IP addresses that SES uses to send your mail, and they can change at any time\.

**Dedicated IP addresses \(standard\)**—you can find the values of the addresses that send your mail in the **Dedicated IPs** page of the SES console\. This is because dedicated IP addresses are static\.

**Dedicated IP addresses \(managed\)**—SES will automatically configure the optimal number of dedicated IP addresses based on your sending patterns\. This means that the dedicated IP addresses in your pool are not visible and will dynamically increase or decrease based upon demand\.