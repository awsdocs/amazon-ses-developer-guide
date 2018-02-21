# Amazon SES Domain Verification Revocation<a name="verified-domain-revocation"></a>

Amazon SES periodically reviews domain verification status, and revokes verification in cases where it is no longer valid\. If Amazon SES is unable to detect the TXT record information required to confirm ownership of a domain, you will receive an **Amazon SES Domain Verification REVOCATION WARNING** email from Amazon SES\.

If you restore the TXT record information to your domain's DNS server within 72 hours, you will receive an **Amazon SES Domain Verification REVOCATION CANCELLATION** email from Amazon SES\.

**Note**  
You can review the required TXT record information in the Amazon SES console by using the following instructions\. In the navigation pane, under **Identity Management**, choose **Domains**\. In the list of domains, choose \(not just expand\) the domain to display the domain verification settings, which include the TXT record name and value\.

If you do not restore the TXT record information to your domain's DNS server within 72 hours, you will receive an **Amazon SES Domain Verification REVOCATION** email from Amazon SES, the domain will be removed from the list of **Verified Senders** on the **Domains** tab, and you will no longer be able to send from the domain\.

To reverify a domain for which verification has been revoked, you must restart the verification procedure from the beginning, just as if the revoked domain were an entirely new domain\.