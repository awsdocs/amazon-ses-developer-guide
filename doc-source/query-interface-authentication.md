# Request Authentication and Amazon SES<a name="query-interface-authentication"></a>

When you make a request to the Amazon SES API, you must provide proof that you are truly the account holder so that Amazon SES can verify your identity and whether you are registered to use services offered by AWS\. If either test fails, Amazon SES returns an error and does not process the request\.

Amazon SES supports signature version 3 and version 4\. Version 4 is preferred\. For information about using signature version 4, see [Signature Version 4 Signing Process](http://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) in the *AWS General Reference*\.