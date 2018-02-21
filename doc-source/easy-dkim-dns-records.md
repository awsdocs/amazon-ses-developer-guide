# Creating DNS Records for DKIM Signing in Amazon SES<a name="easy-dkim-dns-records"></a>

Unlike the Amazon SES console, the Amazon SES API does not generate fully\-formed DNS records for use with DKIM\. Instead, they return DKIM *tokens* — character strings that represent your domain's identity\.

If you are not using the Amazon SES console, you will need to create your own `CNAME` records using the DKIM tokens returned by the API\.

**To create DNS records for DKIM signing**

1. Obtain the DKIM tokens for your domain\. To do so, if you are using the Amazon SES API, call `VerifyDomainDkim` to generate the tokens\. If you already have a DKIM verified identity, call `GetIdentityDkimAttributes` to obtain the tokens\. 

1. In the output from the API, you will receive three DKIM tokens similar to the following:

   ```
   1. vvjuipp74whm76gqoni7qmwwn4w4qusjiainivf6sf
   2. 3frqe7jn4obpuxjpwpolz6ipb3k5nvt2nhjpik2oy
   3. wrqplteh7oodxnad7hsl4mixg2uavzneazxv5sxi2
   ```

1. Use these tokens to construct three `CNAME` records\. For a domain named *example\.com*, the records should appear similar to these:

   ```
   1. vvjuipp74whm76gqoni7qmwwn4w4qusjiainivf6sf._domainkey.example.com CNAME vvjuipp74whm76gqoni7qmwwn4w4qusjiainivf6sf.dkim.amazonses.com
   2. 3frqe7jn4obpuxjpwpolz6ipb3k5nvt2nhjpik2oy._domainkey.example.com CNAME 3frqe7jn4obpuxjpwpolz6ipb3k5nvt2nhjpik2oy.dkim.amazonses.com
   3. wrqplteh7oodxnad7hsl4mixg2uavzneazxv5sxi2._domainkey.example.com CNAME wrqplteh7oodxnad7hsl4mixg2uavzneazxv5sxi2.dkim.amazonses.com
   ```

You can now update your DNS with these records\. Amazon Web Services will eventually detect that you have updated your DNS records; this detection process may take up to 72 hours\. Upon successful detection, you will receive an Amazon SES DKIM Setup Successful confirmation email from Amazon Web Services\. \(Amazon Web Services emails are sent to the email address you used when you signed up for Amazon SES\.\)