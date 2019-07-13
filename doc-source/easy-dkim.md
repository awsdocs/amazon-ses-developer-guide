# Easy DKIM in Amazon SES<a name="easy-dkim"></a>

When you set up Easy DKIM for an identity, Amazon SES automatically adds a 1024\-bit DKIM key to every email that you send from that identity\. You can configure Easy DKIM by using the Amazon SES console, or by using the API\.

**Note**  
To set up Easy DKIM, you have to modify the DNS settings for your domain\. If you use Route 53 as your DNS provider, Amazon SES can automatically create the appropriate records for you\. If you use another DNS provider, see your provider's documentation to learn more about changing the DNS settings for your domain\.

When you successfully configure Easy DKIM, you can start sending email from the DKIM enabled domain, even if you haven't completed the procedures in [Verifying a Domain With Amazon SES](verify-domain-procedure.md)\.

## Easy DKIM Considerations<a name="easy-dkim-considerations"></a>

When you use Easy DKIM to authenticate your email, the following rules apply:
+ You only need to set up Easy DKIM for the domain that you use in your "From" address\. You don't need to set up Easy DKIM for domains that you use in "Return\-Path" or "Reply\-to" addresses\.
+ Amazon SES is available in several AWS Regions\. If you use more than one AWS Region to send email, you have to complete the Easy DKIM setup process in each of those regions to ensure that all of your email is DKIM\-signed\.
+ When you verify a domain, your Easy DKIM settings also apply to all subdomains of that domain, unless you set up Easy DKIM for specific subdomains\.
+ If you set up Easy DKIM for a parent domain, a subdomain, and an email address, Amazon SES applies Easy DKIM settings in the following ways:
  + DKIM settings for a subdomain override the settings for the parent domain\.
  + DKIM settings for an email address override the settings for the subdomain \(if applicable\) and the parent domain\.

**Topics**
+ [Easy DKIM Considerations](#easy-dkim-considerations)
+ [Setting Up Easy DKIM for a Domain](easy-dkim-setup-domain.md)
+ [Setting Up Easy DKIM for an Email Address](easy-dkim-setup-email.md)
+ [Managing Easy DKIM](easy-dkim-managing.md)