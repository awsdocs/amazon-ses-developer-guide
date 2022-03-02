# Using your own IP addresses to send email using Amazon SES<a name="dedicated-ip-byo"></a>

Amazon SES includes a feature called *Bring Your Own IP* \(*BYOIP*\), which makes it possible to use your own IP addresses to send email through Amazon SES\. If you already use a range of IP addresses to send email, you can request that we make your IP range available for sending email through Amazon SES\.

BYOIP is helpful, for example, when you've developed a positive IP reputation using an in\-house email sending system, but you want to migrate to Amazon SES\. By using BYOIP, you can start sending email through Amazon SES immediately, without having to re\-establish the reputations of your IP addresses\.

## Requirements<a name="dedicated-ip-byo-requirements"></a>

To use BYOIP, your IP address range has to meet the following requirements:
+ The address range has to be registered with your Regional internet registry \(RIR\), such as the American Registry for Internet Numbers \(ARIN\), Réseaux IP Européens Network Coordination Centre \(RIPE NCC\), or Asia\-Pacific Network Information Centre \(APNIC\)\. The address range has to be registered to a business or institutional entity and can't be registered to a person\.
+ You have to be able to provide proof that you own the address range by submitting a signed authorization message\.
+ The addresses in the IP address range have to have a clean history\. We might investigate the reputation of the IP address range, and we reserve the right to reject an IP address range if it contains IP addresses that have poor reputations or are associated with malicious behavior\.

## Considerations<a name="dedicated-ip-byo-considerations"></a>

There are several factors that you should consider before you request the transfer of your IP ranges to Amazon SES:
+ The most specific address range that you can specify is /24\. In other words, if you transfer the IP range 203\.0\.113\.0/24 to your Amazon SES account, then you can send from a total of 256 addresses, ranging from 203\.0\.113\.0 to 203\.0\.113\.255\. You have to transfer the entire range—Amazon SES doesn't currently allow you to transfer individual IP addresses\.
+ If you use BYOIP for a specific range of IP addresses, you can only access that range from a single AWS Region\.
+ You can bring five address ranges per Region to your AWS account\.
+ If you use your own IP addresses, you can't use the addresses in the pool of shared Amazon SES IP addresses\. If you need to use these shared IP addresses, you can use Amazon SES in a different AWS Region, or create a new AWS account\.
+ There is a monthly charge for each IP address that you use with BYOIP\. For more information, see [Amazon SES Pricing](https://aws.amazon.com/ses/pricing/)\.

## Using your own IP addresses with Amazon SES<a name="dedicated-ip-byo-request"></a>

In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each BYOIP request carefully\.

If you want to use your own IP range with Amazon SES, send the following information to [ses\-byoip\-request@amazon\.com](mailto:ses-byoip-request@amazon.com):
+ Your AWS account ID\.
+ The AWS Region that you want to use the IP range in, such as ap\-south\-1\.
+ A description of your use case\.
+ The IP range that you want to use with Amazon SES\.
+ The name of the internet registry that the range is registered with\.

 We'll respond to your request within 48 business hours\. In our communications with you, we might request additional information, including documents that prove your ownership of the IP range\.