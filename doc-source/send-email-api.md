# Using the Amazon SES API to send email<a name="send-email-api"></a>

To send production email through Amazon SES, you can use the Simple Mail Transfer Protocol \(SMTP\) interface or the Amazon SES API\. For more information about the SMTP interface, see [Using the Amazon SES SMTP interface to send email](send-email-smtp.md)\. This section describes how to send email by using the API\. 

When you send an email using the Amazon SES API, you specify the content of the message, and Amazon SES assembles a MIME email for you\. Alternatively, you can assemble the email yourself so that you have complete control over the content of the message\. For more information about the API, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\. For a list of endpoint URLs for the AWS Regions where Amazon SES is available, see [Amazon Simple Email Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/ses.html) in the *AWS General Reference*\.

You can call the API in the following ways:
+ **Make direct HTTPS requests—**This is the most advanced method, because you have to manually handle authentication and signing of your requests, and then manually construct the requests\. For information about the Amazon SES API, see the [Welcome](https://docs.aws.amazon.com/ses/latest/APIReference-V2/Welcome.html) page in the *API v2 Reference*\.
+ **Use an AWS SDK—**AWS SDKs make it easy to access the APIs for several AWS services, including Amazon SES\. When you use an SDK, it takes care of authentication, request signing, retry logic, error handling, and other low\-level functions so that you can focus on building applications that delight your customers\.
+ **Use a command line interface—**The [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) is the command line tool for Amazon SES\. We also offer the [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/) for those who script in the PowerShell environment\.

Regardless of whether you access the Amazon SES API directly or indirectly through an AWS SDK, the AWS Command Line Interface or the AWS Tools for Windows PowerShell, the Amazon SES API provides two different ways for you to send an email, depending on how much control you want over the composition of the email message:
+ **Formatted—**Amazon SES composes and sends a properly formatted email message\. You need only supply "From:" and "To:" addresses, a subject, and a message body\. Amazon SES takes care of all the rest\. For more information, see [Sending formatted email using the Amazon SES API](send-email-formatted.md)\.
+ **Raw—**You manually compose and send an email message, specifying your own email headers and MIME types\. If you're experienced in formatting your own email, the raw interface gives you more control over the composition of your message\. For more information, see [Sending raw email using the Amazon SES API](send-email-raw.md)\.

**Topics**
+ [Sending formatted email using the Amazon SES API](send-email-formatted.md)
+ [Sending raw email using the Amazon SES API](send-email-raw.md)
+ [Using templates to send personalized email with the Amazon SES API](send-personalized-email-api.md)
+ [Sending email through Amazon SES using an AWS SDK](send-an-email-using-sdk-programmatically.md)
+ [Content encodings supported by Amazon SES](content-encodings.md)