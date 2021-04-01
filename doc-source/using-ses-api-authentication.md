# Authenticating requests to the Amazon SES API<a name="using-ses-api-authentication"></a>

When you access the Amazon SES API, you authenticate your request using the AWS Signature\. If your request doesn't include a valid signature, Amazon SES returns an error and doesn't process the request\.

For more information about signing AWS requests, see [Signing AWS requests](https://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html) in the *AWS General Reference*\.

**Important**  
Beginning October 1st, 2020, Amazon SES will only support requests signed using Signature Version 4\. If you use an older version of the AWS Signature, you must adopt AWS Signature Version 4 prior to that date\.

## Authentication errors<a name="using-ses-api-authentication-errors"></a>

If you attempt to send email, and your request uses an earlier version of the AWS Signature, you see an error message that resembles the following example:

```
<ErrorResponse xmlns="http://ses.amazonaws.com/doc/2010-12-01/">
  <Error>
    <Type>Sender</Type>
    <Code>InvalidClientTokenId</Code>
    <Message>The security token included in the request is invalid</Message>
  </Error>
</ErrorResponse>
```

You can receive this error if you use an older version of an AWS SDK or the AWS CLI\. You can also see this error if you make a direct HTTPS request to the Amazon SES API that uses an older version of the AWS Signature\.

If you receive this error message, you have to update your requests to use the AWS Signature Version 4\.

### Migrating to the AWS Signature Version 4<a name="using-ses-api-authentication-errors-migrating"></a>

If you use an AWS SDK or the AWS CLI, you should update to the most recent version of the SDK or the AWS CLI\. If you make direct HTTPS requests to the Amazon SES API, update the headers in your requests to use AWS Signature Version 4\. You can easily identify API requests that use earlier versions of the AWS Signature by looking the request headers\. For example, requests that use the AWS Signature Version 3 resemble the following example\.

```
X-Amzn-Authorization: AWS3-HTTPS AWSAccessKeyId=AKIAIOSFODNN7EXAMPLE,Algorithm=HMACSHA256,Signature=lBP67vCvGl ...
```

Requests that use the AWS Signature Version 4 include an `Authorization` header that contains the following information:
+ The algorithm you used for signing \(`AWS4-HMAC-SHA256`\)
+ The credential scope \(with your access key ID\)
+ A list of signed headers
+ The calculated signature\. The signature is based on your request information, and you use your AWS secret access key to produce the signature\. The signature confirms your identity to AWS\.

You can find an example of a call to the Amazon SES API that uses the AWS Signature Version 4 in [Amazon SES API requests](using-ses-api-requests.md)\. For more information about using the AWS Signature Version 4, see [Signature Version 4 signing process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) in the *AWS General Reference*\. 
