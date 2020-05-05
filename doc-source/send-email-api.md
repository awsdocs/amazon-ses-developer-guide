# Using the Amazon SES API to Send Email<a name="send-email-api"></a>

To send production email through Amazon SES, you can use the Simple Mail Transfer Protocol \(SMTP\) interface or the Amazon SES API\. For more information about the SMTP interface, see [Using the Amazon SES SMTP Interface to Send Email](send-email-smtp.md)\. This section describes how to send email by using the API\. 

The Amazon SES API has a Query interface over HTTPS\. When you send an email by using the API, you can information about the content of the email and have Amazon SES assemble the email for you\. Alternatively, you can assemble the email yourself so that you have complete control over the content of the message\. For more information about the API, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\. For a list of endpoint URLs for the AWS Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

You can call the API in the following ways:
+ **Make direct HTTPS requests—**This is the most advanced method, because you have to manually handle authentication and signing of your requests, and then manually construct the requests\. For information about how to make requests, see [Using the Amazon SES API](using-ses-api.md)\.
+ **Use an AWS SDK—**AWS SDKs make it easy to access the APIs for several AWS services, including Amazon SES\. When you use an SDK, it takes care of authentication, request signing, retry logic, error handling, and other low\-level functions so that you can focus on building applications that delight your customers\.
+ **Use a command line interface—**The [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) is the command line tool for Amazon SES\. We also offer the [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/) for those who script in the PowerShell environment\.

Regardless of whether you access the Amazon SES API directly or indirectly through an AWS SDK, the AWS Command Line Interface or the AWS Tools for Windows PowerShell, the Amazon SES API provides two different ways for you to send an email, depending on how much control you want over the composition of the email message:
+ **Formatted—**Amazon SES composes and sends a properly formatted email message\. You need only supply "From:" and "To:" addresses, a subject, and a message body\. Amazon SES takes care of all the rest\. For more information, see [Sending Formatted Email Using the Amazon SES API](send-email-formatted.md)\.
+ **Raw—**You manually compose and send an email message, specifying your own email headers and MIME types\. If you are experienced in formatting your own email, the raw interface gives you more control over the composition of your message\. For more information, see [Sending Raw Email Using the Amazon SES API](send-email-raw.md)\.

**Topics**
+ [Sending Formatted Email Using the Amazon SES API](send-email-formatted.md)
+ [Sending Raw Email Using the Amazon SES API](send-email-raw.md)