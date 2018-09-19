# Amazon SES Domain Verification TXT Records<a name="dns-txt-records"></a>

Your domain is associated with a set of Domain Name System \(DNS\) records that you manage through your DNS provider\. A TXT record is a type of DNS record that provides additional information about your domain\. Each TXT record consists of a name and a value\.

When you initiate domain verification using the Amazon SES console or API, Amazon SES gives you the name and value to use for the TXT record\. For example, if your domain is *example\.com*, the TXT record settings that Amazon SES generates will look similar to the following example:


****  

| Name | Type | Value | 
| --- | --- | --- | 
|  \_amazonses\.example\.com  |  TXT  |  pmBGN/7MjnfhTKUZ06Enqq1PeGUaOkw8lGhcfwefcHU=  | 

Add a TXT record to your domain's DNS server using the specified **Name** and **Value**\. Amazon SES domain verification is complete when Amazon SES detects the existence of the TXT record in your domain's DNS settings\.

If your DNS provider does not allow DNS record names to contain underscores, you can omit *\_amazonses* from the **Name**\. In that case, for the preceding example, the TXT record name would be *example\.com* instead of *\_amazonses\.example\.com*\. To make the record easier to recognize and maintain, you can also optionally prefix the **Value** with *amazonses:*\. In the previous example, the value of the TXT record would therefore be *amazonses:pmBGN/7MjnfhTKUZ06Enqq1PeGUaOkw8lGhcfwefcHU=*\.

**Note**  
Amazon SES previously allowed TXT record names to contain *amazonses* without an underscore\. If you have already verified a domain and your TXT record contains *amazonses* without an underscore, your domain will continue to be verified; there is no action required on your part\. However, any new domains that you verify will require that *amazonses* in the TXT record name is either preceded by an underscore, or *\_amazonses* is removed from the TXT record name entirely\.

You can find troubleshooting information and instructions on how to check your domain verification settings in [Amazon SES Email Address and Domain Verification Problems](domain-verification-problems.md)\. 

## Adding a TXT Record to Your Domain's DNS Server<a name="add-dns-txt-record"></a>

The procedure for adding TXT records to your domain's DNS server depends on who provides your DNS service\. Your DNS provider might be Amazon Route 53 or another domain name registrar such as GoDaddy\. This section provides procedures for adding a TXT record to Route 53, as well as generic procedures that apply to other DNS providers\.

### Procedures for Amazon Route 53<a name="add-dns-txt-record-route53"></a>

When you begin [the process of verifying a new domain](verify-domain-procedure.md) for use with Amazon SES, you can automatically add the domain verification TXT record to your Route 53 configuration\. However, if you choose not to add the TXT record automatically, you can add the TXT record to your Route 53 configuration manually by completing the procedure in this section\.

**To add a TXT record to the DNS record for your Route 53\-managed domain**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Under **Identity Management**, choose **Domains**\.

1. Choose the domain that you want to verify\.

1. Expand the **Verification** section\. Copy the value shown next to **TXT Value**\.

1. Open the Route 53 console at [https://console\.aws\.amazon\.com/route53/](https://console.aws.amazon.com/route53/)\.

1. In the navigation pane, choose **Hosted Zones**\.

1. Select the domain that you want to add a TXT record to, and then choose **Go to Record Sets**\.

1. Choose **Create Record Set**\.

1. In the **Create Record Set** pane, make the following selections:

   1. For **Name**, type **\_amazonses**\.

   1. For **Type**, choose **TXT – Text**\.

   1. For **TTL \(Seconds\)**, type **1800**\.

   1. For **Value**, paste the TXT record value you copied from the Amazon SES console\.

   1. Choose **Create**\.

1. On the **Domains** page in the Amazon SES console, check the value in the **Status** column next to the domain you just attempted to verify\. If the status is "pending verification," wait a few minutes, and then choose **refresh** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/refresh_icon.png)\)\. Repeat this process until the value in the status column is "verified\."

### Generic procedures for other DNS providers<a name="add-dns-txt-record-general"></a>

The procedures for adding TXT records to the DNS configurations vary from provider to provider\. For specific steps, consult your DNS provider's documentation\. The procedure in this section gives a basic overview of the steps you take when adding a TXT record to the DNS configuration for your domain\.

**To add a TXT record to your domain's DNS server \(general procedure\)**

1. Go to your DNS provider's website\. If you aren't sure which DNS provider serves your domain, you can look it up by using a free [ Whois service](http://www.google.com/search?aq=f&hl=en&q=whois+free&btnG=Search)\.

1. On the provider's website, sign in to your account\.

1. Find the page for updating your domain's DNS records\. This page often has a name such as DNS Records, DNS Zone File, or Advanced DNS\. If you're unsure, consult the provider's documentation\.

1. Add a TXT record with the name and value provided by Amazon SES\.
**Important**  
Some DNS providers, such as GoDaddy, automatically append the domain name to the end of DNS records\. Adding a record that already contains the domain name \(such as *\_amazonses\.example\.com*\) might result in the duplication of the domain name \(such as *\_amazonses\.example\.com\.example\.com*\)\. To avoid duplication of the domain name, add a period to the end of the domain name in the DNS record\. This will indicate to your DNS provider that the record name is fully qualified \(that is, no longer relative to the domain name\), and prevent the DNS provider from appending an additional domain name\.

1. Save your changes\. DNS record updates can take up to 48 hours to take effect, but they often take effect much sooner\. You can verify that the TXT record is correctly published by using the procedure in [How to Check Domain Verification Settings](domain-verification-problems.md#domain-verification-check-dns)\.