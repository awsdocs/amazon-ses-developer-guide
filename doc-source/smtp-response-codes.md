# SMTP Response Codes Returned by Amazon SES<a name="smtp-response-codes"></a>

This topic contains a list of SMTP response codes that are returned by Amazon SES\. The way in which errors are handled depends on the SMTP client that you use; some SMTP clients may not display error codes at all\.

You should retry SMTP requests that receive 4xx errors\. In this case, to reduce the likelihood of generating duplicates, we recommend that you implement an exponential retry method with progressively longer waits \(5, 10, and 30 seconds\) between consecutive timeouts\. If the third retry call does not succeed, perform another set of retries after 20 minutes\. For an example implementation that uses an exponential retry policy with Amazon SES, see [How to handle a "Throttling \- Maximum sending rate exceeded" error ](https://aws.amazon.com//blogs/messaging-and-targeting/how-to-handle-a-throttling-maximum-sending-rate-exceeded-error/) on the Amazon SES blog\.

**Note**  
AWS SDKs implement retry logic [automatically](http://docs.aws.amazon.com/general/latest/gr/api-retries.html), although they use the HTTPS interface instead of SMTP\.

SMTP client errors \(5xx\) indicate that you need to revise the request to correct the problem before trying again\. For example, if your AWS authentication credentials are invalid, you must update your setup to use the proper credentials before trying to send the email again\. 


****  

| Description | Response code | More information | 
| --- | --- | --- | 
| Authentication successful | 235 Authentication successful | N/A | 
| Successful delivery | 250 Ok <*Message ID*> | <*Message ID*> is a string of characters that Amazon SES uses to uniquely identify a message\. | 
| Daily sending quota exceeded | 454 Throttling failure: Daily message quota exceeded | You have exceeded the maximum number of emails that Amazon SES permits you to send in a 24\-hour period\. For more information, see [Managing Your Amazon SES Sending Limits](manage-sending-limits.md)\. | 
| Maximum send rate exceeded | 454 Throttling failure: Maximum sending rate exceeded | You have exceeded the maximum number of emails that Amazon SES permits you to send per second\. For more information, see [Managing Your Amazon SES Sending Limits](manage-sending-limits.md)\. | 
| Amazon SES issue when validating SMTP credentials |  454 Temporary authentication failure  | Possible reasons include, but are not limited to:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/smtp-response-codes.html)  | 
| Problem receiving the request |  454 Temporary service failure  | Amazon SES did not successfully receive the request and therefore did not send the message\. Please retry the request\. | 
| Incorrect credentials | 530 Authentication required | Your email\-sending application did not attempt to authenticate with Amazon SES when it tried to connect to the Amazon SES SMTP interface\. For an example of how to set up an email\-sending application to authenticate with Amazon SES, see [Configuring Email Clients to Send Through Amazon SES](configure-email-client.md)\. | 
| Authentication Credentials Invalid | 535 Authentication Credentials Invalid | Your email\-sending application did not provide the correct SMTP credentials to Amazon SES\. Note that your SMTP credentials are not the same as your AWS credentials\. For more information, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\. | 
| Account not subscribed to Amazon SES | 535 Account not subscribed to SES | The AWS account that owns the SMTP credentials is not signed up for Amazon SES\. | 
| User not authorized to call the Amazon SES SMTP endpoint | 554 Access denied: User <User ARN> is not authorized to perform ses:SendRawEmail on resource <Identity ARN> | The AWS Identity and Access Management \(IAM\) policy or the Amazon SES sending authorization policy of the user who owns the SMTP credentials is not allowed to call the Amazon SES SMTP endpoint\. For information about how to get SMTP credentials for an existing IAM user, see [Obtaining Amazon SES SMTP Credentials by Converting AWS Credentials](smtp-credentials.md#smtp-credentials-convert)\. | 
| Unverified email address | 554 Message rejected: Email address is not verified\. The following identities failed the check in region <region>: <identity1>, <identity2>, <identity3> | You are trying to send email from an email address or domain that you have not [verified with Amazon SES](verify-addresses-and-domains.md)\. This error could apply to the "From", "Source", "Sender", or "Return\-Path" address\. If your account is still in the sandbox, you also must verify every recipient email address except for the recipients provided by the [Amazon SES mailbox simulator](mailbox-simulator.md)\. If Amazon SES is not able to show all of the failed identities, the error message ends with an ellipsis\.  Amazon SES has endpoints in [multiple AWS regions](regions.md), and email address verification status is separate for each AWS region\. You must complete the verification process for each sender in the AWS region\(s\) you want to use\.  | 