# Troubleshooting DKIM Problems in Amazon SES<a name="troubleshoot-dkim"></a>

This section lists some of the problems that you may encounter when you configure DKIM authentication in Amazon SES\. If you attempt to set up DKIM and you encounter problems, review the possible causes and solutions below\.

**You set up DKIM successfully, but your messages aren't being DKIM\-signed**  
If you used [Easy DKIM](send-email-authentication-dkim-easy.md) or [BYODKIM](send-email-authentication-dkim-bring-your-own.md) to configure DKIM for a domain, but the messages that you send aren't DKIM\-signed, do the following:  
+ Make sure that DKIM is enabled for the appropriate identity\. To enable DKIM for an identity in the Amazon SES console, choose the email domain in the **Identities** list\. On the details page for the domain, expand **DKIM**, and then choose **Enable** to enable DKIM\.
+ Make sure that you're not sending from a verified email address on the same domain\. If you set up DKIM for a domain, then all of the messages that you send from that domain are DKIM\-signed, *except* for email addresses that you verified individually\. Individually verified email addresses use separate settings\. For example, if you configured DKIM for the domain *example\.com*, and you separately verified the email address *mary@example\.com* \(but didn't configure DKIM for the address\), then emails that you send from *mary@example\.com* are sent without DKIM authentication\. You can resolve this issue by deleting the email address identity from the list of identities for your account\.
+ If you use the same identity in more than one AWS Region, you have to configure DKIM for each region separately\. Similarly, if you use the same domain with more than one AWS account, you have to configure DKIM for each account\. If you remove the necessary DNS records for a specific region or account, Amazon SES disables DKIM signing in that region or account\. If DKIM signing becomes disabled, Amazon SES sends you a notification by email\.

**Your domain's DKIM details in the Amazon SES console show *DKIM: waiting on sender verification\.\.\. DKIM Verification Status: pending verification\.***  
If you complete the procedures in [Easy DKIM](send-email-authentication-dkim-easy.md) or [Provide Your Own DKIM Authentication Token](send-email-authentication-dkim-bring-your-own.md) to configure DKIM for a domain, but the Amazon SES console still indicates that DKIM verification is pending, do the following:  
+ Wait up to 72 hours\. In rare cases, it can take time for the DNS records to become visible to Amazon SES\.
+ Confirm that the CNAME record \(for Easy DKIM\) or the TXT record \(for BYODKIM\) uses the correct name\. Some DNS providers automatically append the domain name to records that you create\. For example, if you create a record with a Name of `example._domainkey.example.com`, your DNS provider might add the name of your domain to the end of this string, resulting in `example._domainkey.example.com.example.com`\. For more information, see the documentation for your DNS provider\.

**You receive an email from Amazon SES that says your DKIM setup has been \(or will be\) revoked\.**  
This means that Amazon SES can no longer find the required CNAME records \(if you used Easy DKIM\) or the required TXT record \(if you used BYODKIM\) records on your DNS server\. The notification email will inform you of the length of time in which you must re\-publish the DNS records before your DKIM setup status is revoked and DKIM signing is disabled\. If your DKIM setup is revoked, you must restart the DKIM set\-up procedure from the beginning\. 

**When attempting to set up BYODKIM, the DKIM verification process fails\.**  
Make sure that your private key uses the right format\. The private key has to be in PKCS \#1 format and use 1024\-bit RSA encryption\. Additionally, the private key has to be base64 encoded\.

**While setting up BYODKIM, you receive a `BadRequestException` error when you try to specify a public key for the domain\.**  
If you receive a `BadRequestException` error, do the following:  
+ Make sure that the selector that you specify for the public key contains at least 1 and less than or equal to 63 alphanumeric characters\. The selector can't include periods or other symbols or punctuation\.
+ Make sure that you've removed the header and footer lines from the public key, and that you've removed all of the line breaks from the public key\.

**When using Easy DKIM, your DNS servers successfully return the Amazon SES DKIM CNAME records, but return `SERVFAIL` for the domain verification TXT record\.**  
Your DNS provider might not be able to redirect CNAME records\. Amazon SES and ISPs query for TXT records\. To comply with the DKIM specification, your DNS servers have to be able to respond to TXT record queries as well as CNAME record queries\. If your DNS provider isn't able to respond to TXT record queries, an alternative is to use Route 53 as your DNS hosting provider\.

**Your emails contain two DKIM signatures**  
The extra DKIM signature, which contains `d=amazonses.com`, is automatically added by Amazon SES\. You can ignore it\.