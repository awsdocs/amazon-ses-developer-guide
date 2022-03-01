# Error codes returned by the Amazon SES API<a name="using-ses-api-error-codes"></a>

This topic contains a list of error codes that are returned by the Amazon SES API\. For more information about the Amazon SES API, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\.

You should retry HTTPS requests that receive 5xx errors\. In this case, to reduce the likelihood of generating duplicates, we recommend that you implement an exponential retry method with progressively longer waits \(5, 10, and 30 seconds\) between consecutive timeouts\. If the third retry call does not succeed, perform another set of retries after 20 minutes\. For an example implementation that uses an exponential retry policy with Amazon SES, see [How to handle a "Throttling \- Maximum sending rate exceeded" error ](https://aws.amazon.com//blogs/messaging-and-targeting/how-to-handle-a-throttling-maximum-sending-rate-exceeded-error/) on the Amazon SES blog\.

**Note**  
AWS SDKs implement retry logic [ automatically](https://docs.aws.amazon.com/general/latest/gr/api-retries.html)\.

HTTPS client errors \(4xx\) indicate that you need to revise the request to correct the problem before trying again\. For example, if your AWS authentication credentials are invalid, you must update your setup to use the proper credentials before trying to send the email again\. 


****  

| Error | Description | HTTPS Status Code | Actions That Return This Code | 
| --- | --- | --- | --- | 
| ConfigurationSetDoesNotExist | The specified configuration set does not exist\. A configuration set is an optional parameter that you use to publish email sending events\. For more information, see [Monitor email sending using Amazon SES event publishing](monitor-using-event-publishing.md)\. | 400 | `SendEmail`, `SendRawEmail` | 
| IncompleteSignature | The request signature does not conform to AWS standards\. | 400 | All | 
| InternalFailure | The request processing has failed because of an unknown error, exception, or failure\. | 500 | All | 
| InvalidAction | The requested action or operation is invalid\. Verify that the action is typed correctly\. | 400 | All | 
| InvalidClientTokenId | The X\.509 certificate or AWS access key ID provided does not exist in our records\. | 403 | All | 
| InvalidParameterCombination | Parameters that must not be used together were used together\. | 400 | All | 
| InvalidParameterValue | An invalid or out\-of\-range value was supplied for the input parameter\. | 400 | All | 
| InvalidQueryParameter | The AWS query string is malformed, does not adhere to AWS standards\. | 400 | All | 
| MailFromDomainNotVerified | The message could not be sent because Amazon SES could not read the MX record required to use the specified MAIL FROM domain\.  | 400 | `SendEmail`, `SendRawEmail` | 
| MalformedQueryString | The query string contains a syntax error\. | 404 | All | 
| MessageRejected | Indicates that the action failed, and the message could not be sent\. Check the error stack for a description of what caused the error\. For more information about problems that can cause this error, see [Amazon SES email sending errors](troubleshoot-error-messages.md)\. | 400 | `SendEmail`, `SendRawEmail` | 
| MissingAction | The request is missing an action or a required parameter\. | 400 | All | 
| MissingAuthenticationToken | The request must contain either a valid \(registered\) AWS access key ID or X\.509 certificate\. | 403 | All | 
| MissingParameter | A required parameter for the specified action is not supplied\. | 400 | All | 
| OptInRequired | The AWS access key ID needs a subscription for the service\. | 403 | All | 
| RequestExpired | The request reached the service more than 15 minutes after the date stamp on the request or more than 15 minutes after the request expiration date \(such as for pre\-signed URLs\), or the date stamp on the request is more than 15 minutes in the future\. | 400 | All | 
| ServiceUnavailable | The request failed due to a temporary failure of the server\. | 503 | All | 
| Throttling | The request was denied due to request throttling\. | 400 | All | 
