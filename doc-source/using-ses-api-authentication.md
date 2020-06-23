# Authenticating requests to the Amazon SES API<a name="using-ses-api-authentication"></a>

When you make a request to the Amazon SES API, you have to prove that you're the account holder so that Amazon SES can verify whether you're registered to use services offered by AWS\. If either test fails, Amazon SES returns an error and doesn't process the request\.

Amazon SES supports AWS Signature Version 4 \(Signature Version 4\)\. For information about using Signature Version 4, see [Signature Version 4 Signing Process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) in the *AWS General Reference*\.

## Authentication errors<a name="using-ses-api-authentication-errors"></a>

Amazon SES previously supported earlier versions of the AWS Signature\. These earlier versions are no longer supported\. Earlier versions of the AWS SDKs used these earlier versions of the AWS Signature\.

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