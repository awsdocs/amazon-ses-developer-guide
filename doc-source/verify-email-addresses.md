# Verifying Email Addresses in Amazon SES<a name="verify-email-addresses"></a>

Amazon SES requires that you verify your *identities* \(the domains or email addresses that you send email from\) to confirm that you own them, and to prevent unauthorized use\. This section includes information about verifying email address identities\. For information about verifying domain identities, see [Verifying Domains in Amazon SES](verify-domains.md)\.

Consider the following factors when you verify email addresses for use with Amazon SES:

+ You must verify each identity that you use as a "From," "Source," "Sender," or "Return\-Path" address\. You can, however, add a label to an email address that has already been verified without performing any additional verification steps \(see the information later in this list\)\.

+ Email addresses are case sensitive\. If you verify *sender@EXAMPLE\.com*, you can't send email from *sender@example\.com* unless you verify *sender@example\.com* as well\.

+ If you verify both an email address and the domain that address belongs to, the settings for the email address override those of the domain\. For example, if DomainKeys Identified Mail \(DKIM\) is enabled for the domain *example\.com*, but not for *sender@example\.com*, emails sent from *sender@example\.com* aren't DKIM signed\.

+ Amazon SES has endpoints in multiple AWS Regions, and the verification status of the email address is separate for each region\. If you want to send email from the same identity in more than one region, you must verify that identity in each region\. For information about using Amazon SES in multiple regions, see [Regions and Amazon SES](regions.md)\.

+ In each AWS Region, you can verify up to 10,000 identities \(email addresses or domains, in any combination\)\.

+ You can add labels to verified email addresses without performing additional verification steps\. To add a label to an email address, add a plus sign \(\+\) between the account name and the "at" sign \(@\), followed by a text label\. For example, if you already verified *sender@example\.com*, you can use *sender\+myLabel@example\.com* as the "From" or "Return\-Path" address for your emails\. You can use this feature to implement Variable Envelope Return Path \(VERP\)\. Then you can use VERP to detect and remove undeliverable email addresses from your mailing lists\. 

+ You can customize the messages that are sent to the email addresses you attempt to verify\. For more information, see [Using Custom Verification Email Templates](custom-verification-emails.md)\.


+ [Verifying an Email Address](verify-email-addresses-procedure.md)
+ [Listing Email Identities in Amazon SES](list-email-addresses-procedure.md)
+ [Deleting an Email Identity in Amazon SES](delete-email-addresses-procedure.md)
+ [Using Custom Verification Email Templates](custom-verification-emails.md)