# Dedicated IP addresses \(standard\) in Amazon SES<a name="manual-dedicated-ips"></a>

*Dedicated IP addresses \(standard\)* are dedicated IP addresses that you manually set up and manage in SES\. They are different from those that are set up and managed automatically using the SES feature [Dedicated IP addresses \(managed\) for Amazon SES](managed-dedicated-sending.md)\. In addition to allowing you full control over your sending reputation using dedicated IP addresses, dedicated IPs \(standard\) enable you to fully manage your dedicated IPs, including warming them up, scaling them out, and IP pool management\.

Dedicated IPs \(standard\) and *Dedicated IPs \(managed\)* both refer to dedicated IP addresses that you lease in SES for [additional pricing](https://aws.amazon.com/ses/pricing), but differ in how they're implemented and managed\. While there are shared benefits common to both, they each have unique advantages to offer depending on your type of email sending, as discussed in [Dedicated IP addresses for Amazon SES](dedicated-ip.md)\.

The topics is this section explain how to manually set up and manage dedicated IPs \(standard\) in SES\.

**Topics**
+ [Requesting and relinquishing dedicated IP addresses \(standard\)](dedicated-ip-case.md)
+ [Warming up dedicated IP addresses \(standard\)](dedicated-ip-warming.md)
+ [Creating standard dedicated IP pools for dedicated IPs \(standard\)](dedicated-ip-pools.md)