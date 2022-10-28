# Managing Easy DKIM and BYODDKIM<a name="send-email-authentication-dkim-easy-managing"></a>

You can manage the DKIM settings for your identities authenticated with either Easy DKIM or BYODKIM by using the web\-based Amazon SES console, or by using the Amazon SES API\. You can use either of these methods to obtain the DKIM records for an identity, or to enable or disable DKIM signing for an identity\. 

## Obtaining DKIM Records for an identity<a name="send-email-authentication-dkim-easy-managing-obtain-records"></a>

You can obtain the DKIM records for your domain or email address at any time by using the Amazon SES console\.

**To obtain the DKIM records for an identity by using the console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the list of identities, choose the identity for which you want to obtain DKIM records\.

1. On the **Authentication** tab of the identity details page, expand **View DNS records**\.

1. Copy either the three CNAME records if you used Easy DKIM, or the TXT record if you used BYODKIM, that appear in this section\. Alternatively, you can choose **Download \.csv record set** to save a copy of the records to your computer\.

   The following image shows an example of the expanded **View DNS records** section revealing CNAME records associated with Easy DKIM\.  
![\[\]](http://docs.aws.amazon.com/ses/latest/dg/images/dkim_existing_dns.png)

You can also obtain the DKIM records for an identity by using the Amazon SES API\. A common method of interacting with the API is to use the AWS CLI\.

**To obtain the DKIM records for an identity by using the AWS CLI**

1. At the command line, type the following command:

   ```
   aws ses get-identity-dkim-attributes --identities "example.com"
   ```

   In the preceding example, replace *example\.com* with the identity that you want to obtain DKIM records for\. You can specify either an email address or a domain\.

1. The output of this command contains a `DkimTokens` section, as shown in the following example:

   ```
   {
       "DkimAttributes": {
           "example.com": {
               "DkimEnabled": true,
               "DkimVerificationStatus": "Success",
               "DkimTokens": [
                   "hirjd4exampled5477y22yd23ettobi",
                   "v3rnz522czcl46quexamplek3efo5o6x",
                   "y4examplexbhyhnsjcmtvzotfvqjmdqoj"
               ]
           }
       }
   }
   ```

   You can use the tokens to create the CNAME records that you add to the DNS settings for your domain\. To create the CNAME records, use the following template:

   ```
   token1._domainkey.example.com CNAME token1.dkim.amazonses.com
   token2._domainkey.example.com CNAME token2.dkim.amazonses.com
   token3._domainkey.example.com CNAME token3.dkim.amazonses.com
   ```

   Replace each instance of *token1* with the first token in the list you received when you ran the `get-identity-dkim-attributes` command, replace all instances of *token2* with the second token in the list, and replace all instances of *token3* with the third token in the list\. 

   For example, applying this template to the tokens shown in the preceding example produces the following records:

   ```
   hirjd4exampled5477y22yd23ettobi._domainkey.example.com CNAME hirjd4exampled5477y22yd23ettobi.dkim.amazonses.com
   v3rnz522czcl46quexamplek3efo5o6x._domainkey.example.com CNAME v3rnz522czcl46quexamplek3efo5o6x.dkim.amazonses.com
   y4examplexbhyhnsjcmtvzotfvqjmdqoj._domainkey.example.com CNAME y4examplexbhyhnsjcmtvzotfvqjmdqoj.dkim.amazonses.com
   ```

**Note**  
If your selected AWS Region is Cape Town, Osaka, or Milan, you will need to use region specific DKIM domains as specified in the [DKIM Domains table](https://docs.aws.amazon.com/general/latest/gr/ses.html) found in the *AWS General Reference*\.

## Disabling Easy DKIM for an identity<a name="send-email-authentication-dkim-easy-managing-disabling"></a>

You can quickly disable DKIM authentication for an identity by using the Amazon SES console\.

**To disable DKIM for an identity**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the list of identities, choose the identity for which you want to disable DKIM\.

1. Under the **Authentication** tab, in the **DomainKeys Identified Mail \(DKIM\)** container, choose **Edit**\.

1. In **Advanced DKIM settings**, clear the **Enabled** box in the **DKIM signatures** field\.

You can also disable DKIM for an identity by using the Amazon SES API\. A common method of interacting with the API is to use the AWS CLI\.

**To disable DKIM for an identity by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses set-identity-dkim-enabled --identity example.com --no-dkim-enabled
  ```

  In the preceding example, replace *example\.com* with the identity that you want to disable DKIM for\. You can specify either an email address or a domain\.

## Enabling Easy DKIM for an identity<a name="send-email-authentication-dkim-easy-managing-enabling"></a>

If you previously disabled DKIM for an identity, you can enable it again by using the Amazon SES console\.

**To enable DKIM for an identity**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the list of identities, choose the identity for which you want to enable DKIM\.

1. Under the **Authentication** tab, in the **DomainKeys Identified Mail \(DKIM\)** container, choose **Edit**\.

1. In **Advanced DKIM settings**, check the **Enabled** box in the **DKIM signatures** field\.

You can also enable DKIM for an identity by using the Amazon SES API\. A common method of interacting with the API is to use the AWS CLI\.

**To enable DKIM for an identity by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses set-identity-dkim-enabled --identity example.com --dkim-enabled
  ```

  In the preceding example, replace *example\.com* with the identity that you want to enable DKIM for\. You can specify either an email address or a domain\.

## Overriding inherited DKIM signing on an email address identity<a name="send-email-authentication-dkim-easy-setup-email"></a>

In this section you'll learn how to override \(disable or enable\) the inherited DKIM signing properties from the parent domain on a specific email address identity that you've already verified with Amazon SES\. You can only do this for email address identities that belong to domains you already own because DNS settings are configured at the domain level\. 

**Important**  
You can't disable/enable DKIM signing for email address identities\.\.\.  
on domains that you don't own\. For example, you can't toggle DKIM signing for a *gmail\.com* or *hotmail\.com* address,
on domains that you own, but have not yet been verified in Amazon SES,
on domains that you own, but have not enabled DKIM signing on the domain\.

This section contains the following topics:
+ [Understanding inherited DKIM signing properties](#dkim-easy-setup-email-key-points-mng)
+ [Overriding DKIM signing on an email address identity \(console\)](#override-dkim-email-console-mng)
+ [Overriding DKIM signing on an email address identity \(AWS CLI\)](#override-dkim-email-cli-mng)

### Understanding inherited DKIM signing properties<a name="dkim-easy-setup-email-key-points-mng"></a>

It's important to first understand that an email address identity inherits its DKIM signing properties from its parent domain if that domain was configured with DKIM, regardless of whether Easy DKIM or BYODKIM was used\. Therefore, disabling or enabling DKIM signing on the email address identity, is in effect, overriding the domain's DKIM signing properties based on these key facts:
+ If you already set up DKIM for the domain that an email address belongs to, you do not need to enable DKIM signing for the email address identity as well\.
  + When you set up DKIM for a domain, Amazon SES automatically authenticates every email from every address on that domain through the inherited DKIM properties from the parent domain\.
+ DKIM settings for a specific email address identity *automatically override the settings of the parent domain or subdomain \(if applicable\)* that the address belongs to\.

Since the email address identity's DKIM signing properties are inherited from the parent domain, if you're planning on overriding these properties, you must keep in mind the hierarchical rules of overriding as explained in the table below\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-dkim-easy-managing.html)

It’s generally never recommended to disable your DKIM signing as it risks tarnishing your sender reputation, and it increases the risk of having your sent mail go to junk or spam folders or having your domain spoofed\.

However, the capability exists to override the domain inherited DKIM signing properties on an email address identity for any particular use case or outlying business decision that you might have to either permanently or temporarily disable DKIM signing, or to re\-enable it at a later time\.

### Overriding DKIM signing on an email address identity \(console\)<a name="override-dkim-email-console-mng"></a>

The following SES console procedure explains how to override \(disable or enable\) the inherited DKIM signing properties from the parent domain on a specific email address identity that you've already verified with Amazon SES\.

**To disable/enable DKIM signing for an email address identity using the console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the list of identities, choose an identity where the **Identity type** is *Email address* and belongs to one of your verified domains\.

1. Under the **Authentication** tab, in the **DomainKeys Identified Mail \(DKIM\)** container, choose **Edit**\.
**Note**  
The **Authentication** tab is only present if the selected email address identity belongs to a domain that has already been verified by SES\. If you haven't verified your domain yet, see [Creating a domain identity](creating-identities.md#verify-domain-procedure)\.

1. Under **Advanced DKIM settings**, in the **DKIM signatures** field, clear the **Enabled** checkbox to disable DKIM signing, or select it to re\-enable DKIM signing \(if it had been overridden previously\)\.

1. Choose **Save changes**\.

### Overriding DKIM signing on an email address identity \(AWS CLI\)<a name="override-dkim-email-cli-mng"></a>

The following example uses the AWS CLI with a SES API command and parameters that will override \(disable or enable\) the inherited DKIM signing properties from the parent domain on a specific email address identity that you've already verified with SES\.

**To disable/enable DKIM signing for an email address identity using the AWS CLI**
+  Assuming you own the *example\.com* domain, and you want to disable DKIM signing for one of the domain's email addresses, at the command line, type the following command:

  ```
  aws sesv2 put-email-identity-dkim-attributes --email-identity marketing@example.com --no-signing-enabled
  ```

  1. Replace *marketing@example\.com* with the email address identity that you want to disable DKIM signing for\.

  1. `--no-signing-enabled` will disable DKIM signing\. To re\-enable DKIM signing, use `--signing-enabled`\.