# Authenticating requests to the Amazon SES API<a name="using-ses-api-authentication"></a>

When you make a request to the Amazon SES API, you have to prove that you're the account holder so that Amazon SES can verify whether you're registered to use services offered by AWS\. If either test fails, Amazon SES returns an error and doesn't process the request\.

Amazon SES supports AWS Signature Version 4\. For information about using Signature Version 4, see [Signature Version 4 Signing Process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) in the *AWS General Reference*\.