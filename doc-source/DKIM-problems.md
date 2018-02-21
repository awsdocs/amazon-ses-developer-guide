# Amazon SES DKIM Problems<a name="DKIM-problems"></a>

If you attempt to set up Easy DKIM using the procedure in [Easy DKIM in Amazon SES](easy-dkim.md) and you encounter problems, review the possible causes and solutions below\.

+ **You set up Easy DKIM successfully, but your messages are not being DKIM\-signed**—Possible problems are:

  + Make sure that Easy DKIM is enabled for the appropriate identity\. To enable Easy DKIM for an identity in the Amazon SES console, choose the email address or domain in the Identities list\. On the Details page for the email address or domain, expand **DKIM**, and then choose **Enable** to enable DKIM\.

  + You could be sending from an individually verified email address that does not have DKIM\-signing enabled\. If you set up Easy DKIM for a domain, it will apply to all email addresses in that domain *except* for email addresses that you individually verified\. Individually verified email addresses use separate settings\. If this is your issue, either remove the email address from your identity list \(its settings will then be inherited from the verified domain's settings\) or enable Easy DKIM for the email address as explained above\.

  + If you are using Amazon SES in multiple regions or with multiple AWS accounts, you must perform the Easy DKIM set\-up procedure described in [Easy DKIM in Amazon SES](easy-dkim.md) for each region and account for which you want to use Easy DKIM\. Amazon SES will generate a unique set of DNS records for each domain/account/region combination\. You will need to add all of these records to your DNS server\. If you remove the necessary DNS records for a specific region or account, Amazon SES will disable DKIM signing only for that account in that region, and notify you by email so that you can take action\. 

+ **Your domain's DKIM details in the Amazon SES console show *DKIM: waiting on sender verification\.\.\. DKIM Verification Status: pending verification***—Your DKIM status is pending, which means that Amazon SES has not yet detected the required CNAME records on your DNS server, which you should have published during the Easy DKIM set\-up procedure \([Easy DKIM in Amazon SES](easy-dkim.md)\)\. If your DKIM status is pending, see the following articles on the Amazon SES blog:

  + [ DKIM Troubleshooting Series: Your DKIM Status is Pending](https://aws.amazon.com//blogs/ses/dkim-troubleshooting-series-your-dkim-status-is-pending/)

  + [ DKIM Troubleshooting Series: Your DKIM Status is Still Pending](https://aws.amazon.com//blogs/ses/dkim-troubleshooting-series-your-dkim-status-is-still-pending/)

+ **When queried, your DNS servers successfully return the Amazon SES DKIM CNAME records, but return SERVFAIL for the TXT records**—Your DNS provider might have problems redirecting CNAME records\. Note that Amazon SES and ISPs query for TXT records\. To comply with the DKIM specification, your DNS servers must be able to respond to TXT record queries as well as CNAME record queries\. If your DNS provider cannot respond to TXT record queries, an alternative is to use Amazon Route 53 for your DNS hosting\. 

+ **Your emails are being DKIM\-signed, but the DKIM signature is not validating**—See [ DKIM Troubleshooting Series: Why is My Signature Not Validating?](https://aws.amazon.com//blogs/ses/dkim-troubleshooting-series-why-is-my-signature-not-validating/) on the Amazon SES blog\.

+ **You receive an email from Amazon SES that says your DKIM setup has been \(or will be\) revoked**—This means that Amazon SES can no longer find the required CNAME records on your DNS server\. The notification email will inform you of the length of time in which you must re\-publish the CNAME records before your DKIM setup status is revoked and DKIM signing is disabled\. If your DKIM setup is revoked, you must restart the DKIM set\-up procedure in [Easy DKIM in Amazon SES](easy-dkim.md) from the beginning\.

+ **You do not have DKIM\-signing enabled, yet your message headers contain a DKIM signature**—The DKIM signature you are seeing contains *d=amazonses\.com* and is automatically added by Amazon SES\.

+ **Your emails contain two DKIM signatures**—The extra DKIM signature, which contains *d=amazonses\.com*, is automatically added by Amazon SES\. You can ignore it\.