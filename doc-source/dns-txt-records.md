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

The procedure for adding TXT records to your domain's DNS server depends on who provides your DNS service\. Your DNS provider might be Route 53 or another domain name registrar such as GoDaddy\. Although we cannot provide specific instructions about how to add TXT records to your domain's DNS server, here is the general procedure DNS providers typically use\.

**To add a TXT record to your domain's DNS server \(general procedure\)**

1. Go to your DNS provider's website\. If you are not sure which DNS provider serves your domain, try looking it up by using a free [ Whois service](http://www.google.com/search?aq=f&hl=en&q=whois+free&btnG=Search)\.

1. Sign in to your domain's account\.

1. Find the page for updating your domain's DNS records\. This page often has a name such as DNS Records, DNS Zone File, Advanced DNS, or something similar\.

1. Locate the TXT records for your domain\.

1. Add a TXT record with the name and value provided by Amazon SES\.
**Important**  
Some DNS providers automatically append the domain name to the end of DNS records\. Adding a record that already contains the domain name \(such as *\_amazonses\.example\.com*\) might result in the duplication of the domain name \(such as *\_amazonses\.example\.com\.example\.com*\)\. To avoid duplication of the domain name, add a period to the end of the domain name in the DNS record\. This will indicate to your DNS provider that the record name is fully qualified \(that is, no longer relative to the domain name\), and prevent the DNS provider from appending an additional domain name\.

1. Save your changes\. DNS record updates can take up to 48 hours to take effect, but they often take effect much sooner\. You can verify that the TXT record is correctly published by using the procedure in [How to Check Domain Verification Settings](domain-verification-problems.md#domain-verification-check-dns)\.