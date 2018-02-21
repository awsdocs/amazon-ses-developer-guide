# Using the Amazon SES SMTP Interface to Send Email<a name="send-email-smtp"></a>

To send production email through Amazon SES, you can use the Simple Mail Transfer Protocol \(SMTP\) interface or the Amazon SES API\. For more information about the Amazon SES API, see [Using the Amazon SES API to Send Email](send-email-api.md)\. This section describes the SMTP interface\.

Amazon SES sends email using the SMTP, the most common email protocol on the Internet\. You can send email through Amazon SES by using a variety of SMTP\-enabled programming languages and software to connect to the Amazon SES SMTP interface\. This section explains how to get your Amazon SES SMTP credentials, how to send email by using the SMTP interface, and how to configure several pieces of software and mail servers to use Amazon SES for email sending\.

**Note**  
For solutions to common problems that you might encounter when you use Amazon SES through its SMTP interface, see [Amazon SES SMTP Issues](smtp-issues.md)\. 

To send email using the Amazon SES SMTP interface, you will need the following:

+ An AWS account\. For more information, see [Signing up for AWS](sign-up-for-aws.md)\.

+ The SMTP interface hostname \(i\.e\., endpoint\)\. For a list of Amazon SES SMTP endpoints, see [Connecting to the Amazon SES SMTP Endpoint](smtp-connect.md)\.

+ The SMTP interface port number\. The port number varies with the connection method\. For more information, see [Connecting to the Amazon SES SMTP Endpoint](smtp-connect.md)\.

+ An SMTP user name and password\. You can use the same set of SMTP credentials in all AWS regions\.
**Important**  
Your SMTP user name and password are not identical to your AWS access keys or the credentials you use to log into the Amazon SES console\. For information about how to generate your SMTP user name and password, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.

+ Client software that can communicate using Transport Layer Security \(TLS\)\. For more information, see [Connecting to the Amazon SES SMTP Endpoint](smtp-connect.md)\.

+ An email address that you have verified with Amazon SES\. For more information, see [Verifying Identities in Amazon SES](verify-addresses-and-domains.md)\.

+ Higher sending limits, if you want to send large quantities of email\. For more information, see [Managing Your Amazon SES Sending Limits](manage-sending-limits.md)\.

Then, you can send email by doing the following:

+ To configure an email client to send email through Amazon SES, including an example for Microsoft Outlook, see [Configuring Email Clients to Send Through Amazon SES](configure-email-client.md)\.

+ To configure SMTP\-enabled software to send email through the Amazon SES SMTP interface, including an example for issue\-tracking software Jira, see [Sending Email Through Amazon SES From Software Packages](send-email-smtp-software-package.md)\.

+ To program an application to send email through Amazon SES, see [Sending Email Through Amazon SES From Your Application](send-email-smtp-app.md)\.

+ To configure your existing email server to send all of your outgoing mail through Amazon SES, see [Integrating Amazon SES with Your Existing Email Server](send-email-smtp-existing-server.md)\.

+ To interact with the Amazon SES SMTP interface using the command line, which can be useful for testing, see [Using the Command Line to Send Email Through the Amazon SES SMTP Interface](send-email-smtp-client-command-line.md)\.

For a list of SMTP response codes, see [SMTP Response Codes Returned by Amazon SES](smtp-response-codes.md)\.

## Email Information to Provide<a name="smtp-parameters"></a>

When you access Amazon SES through the SMTP interface, your SMTP client application assembles the message, so the information you need to provide depends on the application you are using\. At a minimum, the SMTP exchange between a client and a server requires a source address, a destination address, and message data\.

If you are using the SMTP interface and have feedback forwarding enabled, then your bounces, complaints, and delivery notifications are sent to the "MAIL FROM" address\. Any "Reply\-To" address that you specify is not used\.