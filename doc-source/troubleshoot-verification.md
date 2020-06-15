# Amazon SES Email Address and Domain Verification Problems<a name="troubleshoot-verification"></a>

To verify an email address or domain with Amazon SES, you initiate the process using either the Amazon SES console or the Amazon SES API\. This section contains information that may help resolve issues with the verification process\.

## Common Email Verification Problems<a name="troubleshoot-verification-email"></a>
+ **The verification email didn't arrive** – If you complete the procedures in [Verifying email addresses in Amazon SES](verify-email-addresses.md) but you don't receive the verification email within a few minutes, complete the following steps:
  + Check the spam or junk mail folder for the email address you're attempting to verify\.
  + Confirm that the address that you're trying to verify is able to receive email\. Using a separate email address \(such as your personal email address\), send a test email to the address that you want to verify\.
  + Check [the list of verified addresses in the Amazon SES console](https://console.aws.amazon.com/ses/home#verified-senders-email:)\. Make sure that there aren't any errors in the email address that you're attempting to verify\.

## Common Domain Verification Problems<a name="troubleshoot-verification-domain"></a>

If you attempt to verify a domain using the procedure in [Verifying domains in Amazon SES](verify-domains.md) and you encounter problems, review the possible causes and solutions below\.
+ **You're attempting to verify a domain that you don't own** – You can't verify a domain that you don't own\. For example, if you want to send email through Amazon SES from an address on the *gmail\.com* domain, you need to [verify that email address specifically](verify-email-addresses.md)\. You can't verify the entire *gmail\.com* domain\.
+ **Your DNS provider doesn't allow underscores in TXT record names** – Some DNS providers don't allow you to include the underscore character in the DNS record names for your domain\. If this is true for your provider, you can omit *\_amazonses* from the name of the TXT record\.
+ **Your DNS provider appended the domain name to the end of the TXT record** – Some DNS providers automatically append the name of your domain to the attribute name of TXT record\. For example, if you create a record where the attribute name is *\_amazonses\.example\.com*, the provider might append the domain name, resulting in *\_amazonses\.example\.com\.example\.com*\)\. To avoid duplication of the domain name, add a period to the end of the domain name when you create the TXT record\. This step tells your DNS provider that it isn't necessary to append the domain name to the TXT record\.
+ **Your DNS provider modified the DNS record value** – Some providers automatically modify DNS record values to use only lowercase letters\. Amazon SES only verifies your domain when it detects a verification record for which the attribute value exactly matches the value that Amazon SES provided when you started the domain verification process\. If the DNS provider for your domain changes your TXT record values to use only lowercase letters, contact the DNS provider for additional assistance\.
+ **You want to verify the same domain multiple times** – You might need to verify your domain more than once because you're sending in different regions, or because you're using the same domain to send from multiple AWS accounts\. If your DNS provider doesn't allow you to have more than one TXT record with the same attribute name, you might still be able to verify two domains\. If your DNS provider allows it, you can assign multiple attribute values to the same TXT record\. For example, if your DNS is managed by Amazon Route 53, you can set up multiple values for the same TXT record by completing the following steps: 

  1. In the Route 53 console, choose the TXT record you created when you verified your domain in the first region\.

  1. In the **Value** box, go to the end of the existing attribute value, and then press Enter\.

  1. Add the attribute value for the additional region, and then save the record set\.

  If your DNS provider doesn't let you to assign multiple values to the same TXT record, you can verify the domain once with *\_amazonses* in the attribute name of the TXT record, and another time with *\_amazonses* removed from the attribute name\. The downside of this solution is that you can only verify the same domain two times\.

## How to Check Domain Verification Settings<a name="troubleshoot-verification-domain-dns"></a>

You can check that your Amazon SES domain verification TXT record is published correctly to your DNS server by using the following procedure\. This procedure uses the [nslookup](http://en.wikipedia.org/wiki/Nslookup) tool, which is available for Windows and Linux\. On Linux, you can also use [dig](http://en.wikipedia.org/wiki/Dig_(command))\.

The commands in these instructions were executed on Windows 7, and the example domain we use is *ses\-example\.com*\.

In this procedure, you first find the DNS servers that serve your domain, and then query those servers to view the TXT records\. You query the DNS servers that serve your domain because those servers contain the most up\-to\-date information for your domain, which can take time to propagate to other DNS servers\.

**To verify that your domain verification TXT record is published to your DNS server**

1. Find the name servers for your domain by taking the following steps\.

   1. Go to the command line\. To get to the command line on Windows 7, choose **Start** and then type **cmd**\. On Linux\-based operating systems, open a terminal window\.

   1. At the command prompt, type the following, where *<domain>* is your domain\. This will list all of the name servers that serve your domain\. 

      ```
      1. nslookup -type=NS <domain>
      ```

      If your domain was *ses\-example\.com*, this command would look like:

      ```
      1. nslookup -type=NS ses-example.com
      ```

      The command's output will list the name servers that serve your domain\. You will query one of these servers in the next step\.

1. Verify that the TXT record is correctly published by taking the following steps\. 

   1. At the command prompt, type the following, where *<domain>* is your domain, and *<name server>* is one of the name servers you found in step 1\.

      ```
      1. nslookup -type=TXT  _amazonses.<domain> <name server>
      ```

      In our *ses\-example\.com* example, if a name server that we found in step 1 was called *ns1\.name\-server\.net*, we would type the following:

      ```
      1. nslookup -type=TXT  _amazonses.ses-example.com ns1.name-server.net
      ```

   1. In the output of the command, verify that the string that follows `text = ` matches the TXT value you see when you choose the domain in the Identities list of the Amazon SES console\. 

      In our example, we are looking for a TXT record under *\_amazonses\.ses\-example\.com* with a value of `fmxqxT/icOYx4aA/bEUrDPMeax9/s3frblS+niixmqk=`\. If the record is correctly published, we would expect the command to have the following output:

      ```
      1. _amazonses.ses-example.com text = "fmxqxT/icOYx4aA/bEUrDPMeax9/s3frblS+niixmqk="
      ```