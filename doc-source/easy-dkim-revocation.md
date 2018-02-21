# DKIM Record Revocation in Amazon SES<a name="easy-dkim-revocation"></a>

Amazon Web Services periodically reviews DKIM DNS records, and revokes DKIM signing in cases where it is no longer valid\. If Amazon Web Services is unable to detect the CNAME record information required to confirm the ownership of a domain, you will receive an *Amazon SES DKIM REVOCATION WARNING* email from Amazon Web Services\. Amazon SES will continue to send your email, but it will not be signed using a DKIM signature\.

If you restore the CNAME record information to your DNS settings within five days, you will receive an *Amazon SES DKIM REVOCATION CANCELLATION* email from Amazon Web Services\. Amazon SES will once again sign email using a DKIM signature from a verified identity for which you have enabled Easy DKIM\.

If you do not restore the CNAME record information to your DNS settings within five days, you will receive an *Amazon SES DKIM REVOCATION* email from Amazon Web Services, and email you send via Amazon SES will not be signed using a DKIM signature\. 

To set up Easy DKIM for a domain for which DKIM signing has been revoked, you must restart the procedure from the beginning, as if you were setting up Easy DKIM for the first time\.