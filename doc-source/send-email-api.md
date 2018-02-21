# Using the Amazon SES API to Send Email<a name="send-email-api"></a>

To send production email through Amazon SES, you can use the Simple Mail Transfer Protocol \(SMTP\) interface or the Amazon SES API\. For more information about the SMTP interface, see [Using the Amazon SES SMTP Interface to Send Email](send-email-smtp.md)\. This section describes how to send email by using the API\. 

The Amazon SES API has a Query interface over HTTPS\. See [Regions and Amazon SES](regions.md) for a list of Amazon SES API endpoints\. When you send an email by using the API, you can provide limited information and have Amazon SES assemble the email for you, or you can assemble the email yourself so that you have complete control over the content and formatting\. For more information about the API, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/)\. You can call the API in the following ways:

+ **Make raw Query requests and responses—**This is the most advanced method, because you are calling the API directly\. For information about how to make Query requests and responses, see [Amazon SES Query API](query-interface.md)\.

+ **Use an AWS SDK—**AWS SDKs wrap the low\-level functionality of the Amazon SES API with higher\-level data types and function calls that take care of the details for you, and provide basic functions \(not included in the Amazon SES API\), such as request authentication, request retries, and error handling\.

+ **Use a command line interface—**The [AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) is the command line tool for Amazon SES\. We also offer the [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/) for those who script in the PowerShell environment\.

Regardless of whether you access the Amazon SES API directly or indirectly through an AWS SDK, the AWS Command Line Interface or the AWS Tools for Windows PowerShell, the Amazon SES API provides two different ways for you to send an email, depending on how much control you want over the composition of the email message:

+ **Formatted—**Amazon SES composes and sends a properly formatted email message\. You need only supply "From:" and "To:" addresses, a subject, and a message body\. Amazon SES takes care of all the rest\. For more information, see [Sending Formatted Email Using the Amazon SES API](send-email-formatted.md)\.

+ **Raw—**You manually compose and send an email message, specifying your own email headers and MIME types\. If you are experienced in formatting your own email, the raw interface gives you more control over the composition of your message\. For more information, see [Sending Raw Email Using the Amazon SES API](send-email-raw.md)\.