# Amazon SES Email Address and Domain Verification Problems<a name="domain-verification-problems"></a>

To verify an email address or domain with Amazon SES, you initiate the process using either the Amazon SES console or the Amazon SES API\. This section contains information that may help resolve issues with the verification process\.

## Common Email Verification Problems<a name="email-verification-common-problems"></a>

### The Verification Email Did Not Arrive<a name="troubleshooting-verification-email-did-not-arrive"></a>

If you complete the procedures in [Verifying Email Addresses in Amazon SES](verify-email-addresses.md) but do not receive the verification email within a few minutes, complete the following troubleshooting tasks:

+ Check the spam or junk mail folder in your email client\.

+ Confirm that the address you are trying to verify is able to receive email\. Send a test email to the address that you want to verify from a separate email address\.

+ Check [the list of verified addresses in the Amazon SES console](https://console.aws.amazon.com/ses/home#verified-senders-email:)\. Confirm that there are no errors in the email address you want to verify\.

## Common Domain Verification Problems<a name="domain-verification-common-problems"></a>

If you attempt to verify a domain using the procedure in [Verifying Domains in Amazon SES](verify-domains.md) and you encounter problems, review the possible causes and solutions below\.

+ **Your DNS provider does not allow underscores in TXT record names**—You can omit *\_amazonses* from the TXT record name\.

+ **You want to verify the same domain multiple times and you can't have multiple TXT records with the same name**—You might need to verify your domain more than once because you're sending in different regions or you're sending from multiple AWS accounts from the same domain in the same region\. If your DNS provider does not allow you to have multiple TXT records with the same name, there are two workarounds\. The first workaround, if your DNS provider allows it, is to assign multiple values to the TXT record\. For example, if your DNS is managed by Amazon Route 53, you can set up multiple values for the same TXT record as follows: 

  1. In the Route 53 console, choose the *\_amazonses* TXT record you added when you verified your domain in the first region\.

  1. In the **Value** box, press Enter after the first value\.

  1. Add the value for the additional region, and save the record set\.

  The other workaround is that if you only need to verify your domain twice, you can verify it once with *\_amazonses* in the TXT record name and the other time you can omit *\_amazonses* from the record name entirely\. We recommend the previous solution as a best practice, however\.

+ **Your email address is provided by a web\-based email service you do not have control over**—You cannot successfully verify a domain that you do not own\. For example, if you want to send email through Amazon SES from a gmail address, you need to verify that email address specifically; you cannot verify gmail\.com\. For information about individual email address verification, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)\. 

+ **Amazon SES reports that domain verification failed**—You receive a "Domain Verification Failure" email from Amazon SES, and the domain displays a status of "failed" in the **Domains** tab of the Amazon SES console\. This means that Amazon SES cannot find the necessary TXT record on your DNS server\. Verify that the required TXT record is correctly published to your DNS server by using the procedure in [How to Check Domain Verification Settings](#domain-verification-check-dns), and look for the following possible errors:

  + **Your DNS provider appended the domain name to the end of the TXT record**—Adding a TXT record that already contains the domain name \(such as *\_amazonses\.example\.com*\) may result in the duplication of the domain name \(such as *\_amazonses\.example\.com\.example\.com*\)\. To avoid duplication of the domain name, add a period to the end of the domain name in the TXT record\. This will indicate to your DNS provider that the record name is fully qualified \(that is, no longer relative to the domain name\), and prevent the DNS provider from appending an additional domain name\.

+ **You receive an email from Amazon SES that says your domain verification has been \(or will be\) revoked**—Amazon SES can no longer find the required TXT record on your DNS server\. The notification email will inform you of the length of time in which you must re\-publish the TXT record before your domain verification status is revoked\.
**Note**  
You can review the required TXT record information in the Amazon SES console by using the following instructions\. In the navigation pane, under **Identities**, choose **Domains**\. In the list of domains, choose \(not just expand\) the domain to display the domain verification settings, which include the TXT record name and value\.

  If your domain verification status is revoked, you must restart the verification procedure in [Verifying Domains in Amazon SES](verify-domains.md) from the beginning, just as if the revoked domain were an entirely new domain\. After you publish the TXT record to your DNS server, verify that the TXT record is correctly published by using [How to Check Domain Verification Settings](#domain-verification-check-dns)\. 

## How to Check Domain Verification Settings<a name="domain-verification-check-dns"></a>

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