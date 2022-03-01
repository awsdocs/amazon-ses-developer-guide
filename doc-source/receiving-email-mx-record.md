# Publishing an MX record for Amazon SES email receiving<a name="receiving-email-mx-record"></a>

A *mail exchanger* record \(*MX record*\) is a configuration that specifies which mail servers can accept email that's sent to your domain\. 

To have Amazon SES manage your incoming email, you need to add an MX record to your domain's DNS configuration\. The MX record that you create refers to the endpoint that receives email for the AWS Region where you use Amazon SES\. For example, the endpoint for the US West \(Oregon\) Region is *inbound\-smtp\.us\-west\-2\.amazonaws\.com*\. For a complete list of endpoints, see [Amazon SES regions and endpoints](regions.md#region-endpoints)\.

**Note**  
The endpoints that receive email in Amazon SES aren't IMAP or POP3 email servers\. You can't use these URLs as incoming mail servers in email clients\.  
If you need a solution that can both send and receive email by using an email client, consider using [Amazon WorkMail](https://aws.amazon.com/workmail)\.

The following procedure includes general steps for creating an MX record\. The specific procedures for creating an MX record depend on your DNS or hosting provider\. See your provider's documentation for information about adding an MX record to the DNS configuration for your domain\.

**Note**  
To complete the following procedure, you have to be able to modify the DNS records for your domain\. If you can't access the DNS records for your domain, or you're not comfortable doing so, contact your system administrator for assistance\.

**To add an MX record to the DNS configuration for your domain**

1. Sign in to the management console for your DNS provider\.

1. Create a new MX record\.

1. For the MX record **Name**, enter your domain, followed by a period\. For example, if you want Amazon SES to manage email that's sent to the domain *example\.com*, enter the following:

   ```
   example.com.
   ```
**Note**  
Some DNS providers refer to the **Name** field as the **Host**, **Domain**, or **Mail Domain**\.

1. For **Type**, choose **MX**\.
**Note**  
Some DNS providers refer to the **Type** field as the **Record Type** or a similar name\.

1. For **Value**, enter the following:

   ```
   10 inbound-smtp.regionInboundUrl.amazonaws.com
   ```

   In the preceding example, replace *regionInboundUrl* with the address of the endpoint that receives email for the AWS Region you use with Amazon SES\. For example, if you're using the US East \(N\. Virginia\) Region, replace *region* with `us-east-1`\. For a complete list of email receiving endpoints, see [Amazon SES regions and endpoints](regions.md#region-endpoints)\.
**Note**  
The management consoles of some DNS providers include separate fields for the record **Value** and the record **Priority**\. If this is the case for your DNS provider, enter `10` for the **Priority** value, and enter the incoming mail endpoint URL for the **Value**\.

## Instructions for creating MX records for various providers<a name="receiving-email-mx-record-links"></a>

The procedures for creating an MX record for your domain depend on which DNS provider you use\. This section includes links to the documentation for several common DNS providers\. This list isn't a complete list of providers\. If your provider isn't listed below, you can probably still use it with Amazon SES\. Inclusion on this list isn’t an endorsement or recommendation of any company’s products or services\.


| DNS/Hosting Provider Name | Documentation Link | 
| --- | --- | 
|  Amazon Route 53  |  [Creating Records by Using the Amazon Route 53 Console](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-creating.html)  | 
|  GoDaddy  |  [Add an MX record](https://www.godaddy.com/help/add-an-mx-record-19234)  | 
|  DreamHost  |  [How do I change my MX records?](https://help.dreamhost.com/hc/en-us/articles/215035328)  | 
|  Cloudflare  |  [How do I add or edit mail or MX records? ](https://support.cloudflare.com/hc/en-us/articles/218069617-How-do-I-add-or-edit-mail-or-MX-records-)  | 
|  HostGator  |  [Changing MX records \- Windows](https://support.hostgator.com/articles/hosting-guide/lets-get-started/dns-name-servers/changing-mx-records-windows)  | 
|  Namecheap  |  [How can I set up MX records required for mail service? ](https://www.namecheap.com/support/knowledgebase/article.aspx/322/2237/how-can-i-set-up-mx-records-required-for-mail-service)  | 
|  Names\.co\.uk  |  [Changing your domain's DNS settings](https://www.names.co.uk/support/articles/changing-your-domains-dns-settings/)  | 
|  Wix  |  [Adding or Updating MX Records in Your Wix Account](https://support.wix.com/en/article/adding-or-updating-mx-records-in-your-wix-account)  | 


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 
