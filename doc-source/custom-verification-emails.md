# Using Custom Verification Email Templates<a name="custom-verification-emails"></a>

When you attempt to verify an email address, Amazon SES sends an email to that address that resembles the example shown in the following image\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/verification_email_example.png)

Several Amazon SES customers build applications \(such as email marketing suites or ticketing systems\) that send email through Amazon SES on behalf of their own customers\. For the end users of these applications, the email verification process can be confusing: the verification email uses Amazon SES branding, rather than the branding of the application, and those end users never signed up to use Amazon SES directly\.

If your Amazon SES use case requires your customers to have their email addresses verified for use with Amazon SES, you can create customized verification emails\. These customized emails help reduce customer confusion and increase the rates at which your customers complete the registration process\.


+ [Creating a Custom Verification Email Template](#custom-verification-emails-creating)
+ [Editing a Custom Verification Email Template](#custom-verification-emails-editing)
+ [Sending Verification Emails Using Custom Templates](#custom-verification-emails-sending)
+ [Custom Verification Email Frequently Asked Questions](#custom-verification-emails-faq)

## Creating a Custom Verification Email Template<a name="custom-verification-emails-creating"></a>

To create a custom verification email, use the `CreateCustomVerificationEmailTemplate` API operation\. This operation takes the following inputs:


****  

| Attribute | Description | 
| --- | --- | 
| TemplateName | The name of the template\. The name you specify must be unique\. | 
| FromEmailAddress | The email address that the verification email is sent from\. The address or domain you specify must be verified for use with your Amazon SES account\. | 
| TemplateSubject | The subject line of the verification email\. | 
| TemplateContent | The body of the email\. The email body can contain HTML, with certain limitations\. For more information, see [Custom Verification Email Frequently Asked Questions](#custom-verification-emails-faq)\. | 
| SuccessRedirectionURL | The URL that users are sent to if their email addresses are successfully verified\. | 
| FailureRedirectionURL | The URL that users are sent to if their email addresses are not successfully verified\. | 

You can use the AWS SDKs or the AWS CLI to create a custom verification email template with the `CreateCustomVerificationEmailTemplate` operation\. To learn more about the AWS SDKs, see [Tools for Amazon Web Services](https://aws.amazon.com/tools/#sdk)\. For more information about the AWS CLI, see [AWS Command Line Interface\.](https://aws.amazon.com/cli)

The following section includes procedures for creating a custom verification email using the AWS CLI\. These procedures assume that you have installed and configured the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](http://docs.aws.amazon.com/cli/latest/userguide/)\.

**Note**  
To complete the procedure in this section, you must use version 1\.14\.6 or later of the AWS CLI\. For best results, upgrade to the latest version of the AWS CLI\. For more information about updating the AWS CLI, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the AWS Command Line Interface User Guide\.

1. In a text editor, create a new file\. Paste the following content into the editor:

   ```
   {
     "TemplateName": "SampleTemplate",
     "FromEmailAddress": "sender@example.com",
     "TemplateSubject": "Please confirm your email address",
     "TemplateContent": "<html>
                         <head></head>
                         <body style="font-family:sans-serif;">
                           <h1 style="text-align:center">Ready to start sending 
                           email with ProductName?</h1>
                           <p>We here at Example Corp are happy to have you on
                             board! There's just one last step to complete before
                             you can start sending email. Just click the following
                             link to verify your email address. Once we confirm that 
                             you're really you, we'll give you some additional 
                             information to help you get started with ProductName.</p>
                         </body>
                         </html>",
     "SuccessRedirectionURL": "https://www.example.com/verifysuccess",
     "FailureRedirectionURL": "https://www.example.com/verifyfailure"
   }
   ```
**Important**  
To make the preceding example easier to read, the `TemplateContent` attribute contains line breaks\. If you paste the preceding example into your text file, remove the line breaks before proceeding\.

   Replace the values of `TemplateName`, `FromEmailAddress`, `TemplateSubject`, `TemplateContent`, `SuccessRedirectionURL`, and `FailureRedirectionURL` with your own values\. Save the file as `customverificationemail.json`\.

1. At the command line, type the following command to create the custom verification email template: aws ses create\-custom\-verification\-email\-template \-\-cli\-input\-json file://customverificationemail\.json

1. Optionally, you can confirm that the template was created by typing the following command: aws ses list\-custom\-verification\-email\-templates

## Editing a Custom Verification Email Template<a name="custom-verification-emails-editing"></a>

You can edit a custom verification email template using the `UpdateCustomVerificationEmailTemplate` operation\. This operation accepts the same inputs as the `CreateCustomVerificationEmailTemplate` operation \(that is, the `TemplateName`, `FromEmailAddress`, `TemplateSubject`, `TemplateContent`, `SuccessRedirectionURL`, and `FailureRedirectionURL` attributes\)\. However, with the `UpdateCustomVerificationEmailTemplate` operation, none of these attributes are required\. When you pass a value for `TemplateName` that is the same as the name of an existing custom verification email template, the attributes you specify overwrite the attributes that were originally in the template\.

## Sending Verification Emails Using Custom Templates<a name="custom-verification-emails-sending"></a>

After you create at least one custom verification email template, you can send it to your customers by calling the [SendCustomVerificationEmail](http://docs.aws.amazon.com/ses/latest/APIReference/API_SendCustomVerificationEmail.html) API operation\. You can call the `SendCustomVerificationEmail` operation by using any of the AWS SDKs or the AWS CLI\. The `SendCustomVerificationEmail` operation takes the following inputs:


****  

| Attribute | Description | 
| --- | --- | 
| EmailAddress | The email address that is being verified\. | 
| TemplateName | The name of the custom verification email template that is sent to email address that is being verified\. | 
| ConfigurationSetName | \(Optional\) The name of a configuration set to use when sending the verification email\. | 

For example, assume your customers register for your service using a form in your application\. When the customer completes the form and submits it, your application calls the `SendCustomVerificationEmail` operation, passing the customer's email address and the name of the template you want to use\. 

Your customer receives an email that uses the customized email template you created\. Amazon SES automatically adds a unique link to the recipient, as well as a brief disclaimer\. The following image shows a sample verification email that uses the template created in [Creating a Custom Verification Email Template](#custom-verification-emails-creating)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/cve_sample_message.png)

## Custom Verification Email Frequently Asked Questions<a name="custom-verification-emails-faq"></a>

This section contains answers to frequently asked questions about the custom verification email template feature\.

### Q1\. How many custom verification email templates can I create?<a name="custom-verification-emails-faq-q1"></a>

You can create up to 50 custom verification email templates per Amazon SES account\.

### Q2\. How do custom verification emails appear to recipients?<a name="custom-verification-emails-faq-q2"></a>

Custom verification emails include the content you specified when you created the template, followed by a link that recipients must click to verify their email addresses\.

### Q3\. Can I preview the custom verification email?<a name="custom-verification-emails-faq-q3"></a>

To preview a custom verification email, use the `SendCustomVerificationEmail` operation to send a verification email to an address you own\. If you do not click the verification link, Amazon SES does not create a new identity\. If you do click the verification link, you can optionally delete the newly created identity using the `DeleteIdentity` operation\.

### Q4\. Can I include images in my custom verification email templates?<a name="custom-verification-emails-faq-q4"></a>

You can embed images in the HTML for your templates by using Base64 encoding\. When you embed images in this way, Amazon SES automatically converts them into attachments\. You can encode an image at the command line by issuing one of the following commands:

+ Linux, macOS, or Unix: base64 \-i *imagefile\.png* | tr \-d '\\n' > output\.txt

+ Windows: certutil \-encode *imagefile\.png* output\.tmp && findstr /v /c:\- output\.tmp > output\.txt && del output\.tmp

Replace `imagefile.png` with the name of the file you want to encode\. In both of the commands above, the Base64 encoded image is saved to `output.txt`\.

**Note**  
If you encoded the image using the Windows command line, you must open `output.txt` in a text editor and remove the line breaks from the file before proceeding\.

You can embed the Base64\-encoded image by including the following in the HTML for the template: `<img src="data:image/png;base64,base64EncodedImage"/>`

In the example above, replace `png` with the file type of the encoded image \(such as jpg or gif\), and replace `base64EncodedImage` with the Base64 encoded image \(that is, the contents of `output.txt` from one of the preceding commands\)\.

### Q5\. Are there any limits to the content that I can include in custom verification email templates?<a name="custom-verification-emails-faq-q5"></a>

Custom verification email templates may not exceed 10 MB in size\. Additionally, custom verification email templates that contain HTML can only use the tags and attributes listed in the following table:


| HTML tag | Allowed attributes | 
| --- | --- | 
| abbr | class, id, style, title | 
| acronym | class, id, style, title | 
| address | class, id, style, title | 
| area | class, id, style, title | 
| b | class, id, style, title | 
| bdo | class, id, style, title | 
| big | class, id, style, title | 
| blockquote | cite, class, id, style, title | 
| body | class, id, style, title | 
| br | class, id, style, title | 
| button | class, id, style, title | 
| caption | class, id, style, title | 
| center | class, id, style, title | 
| cite | class, id, style, title | 
| code | class, id, style, title | 
| col | class, id, span, style, title, width | 
| colgroup | class, id, span, style, title, width | 
| dd | class, id, style, title | 
| del | class, id, style, title | 
| dfn | class, id, style, title | 
| dir | class, id, style, title | 
| div | class, id, style, title | 
| dl | class, id, style, title | 
| dt | class, id, style, title | 
| em | class, id, style, title | 
| fieldset | class, id, style, title | 
| font | class, id, style, title | 
| form | class, id, style, title | 
| h1 | class, id, style, title | 
| h2 | class, id, style, title | 
| h3 | class, id, style, title | 
| h4 | class, id, style, title | 
| h5 | class, id, style, title | 
| h6 | class, id, style, title | 
| head | class, id, style, title | 
| hr | class, id, style, title | 
| html | class, id, style, title | 
| i | class, id, style, title | 
| img | align, alt, class, height, id, src, style, title, width | 
| input | class, id, style, title | 
| ins | class, id, style, title | 
| kbd | class, id, style, title | 
| label | class, id, style, title | 
| legend | class, id, style, title | 
| li | class, id, style, title | 
| map | class, id, style, title | 
| menu | class, id, style, title | 
| ol | class, id, start, style, title, type | 
| optgroup | class, id, style, title | 
| option | class, id, style, title | 
| p | class, id, style, title | 
| pre | class, id, style, title | 
| q | cite, class, id, style, title | 
| s | class, id, style, title | 
| samp | class, id, style, title | 
| select | class, id, style, title | 
| small | class, id, style, title | 
| span | class, id, style, title | 
| strike | class, id, style, title | 
| strong | class, id, style, title | 
| sub | class, id, style, title | 
| sup | class, id, style, title | 
| table | class, id, style, summary, title, width | 
| tbody | class, id, style, title | 
| td | abbr, axis, class, colspan, id, rowspan, style, title, width | 
| textarea | class, id, style, title | 
| tfoot | class, id, style, title | 
| th | abbr, axis, class, colspan, id, rowspan, scope, style, title, width | 
| thead | class, id, style, title | 
| tr | class, id, style, title | 
| tt | class, id, style, title | 
| u | class, id, style, title | 
| ul | class, id, style, title, type | 
| var | class, id, style, title | 

### Q6\. How many verified email addresses can exist in my account?<a name="custom-verification-emails-faq-q6"></a>

Your Amazon SES account can have up to 10,000 verified identities in each AWS Region\. In Amazon SES, *identities* include both verified domains and email addresses\. If you have verified domains or email addresses for your own email sending, those identities are included in the 10,000 identity limit\.

### Q7\. Can I create custom verification email templates using the Amazon SES console?<a name="custom-verification-emails-faq-q7"></a>

Currently, it is only possible to create, edit, and delete custom verification emails using the Amazon SES API\.

### Q8\. Can I track open and click events that occur when customers receive custom verification emails?<a name="custom-verification-emails-faq-q8"></a>

Custom verification emails cannot include open or click tracking\.

### Q9\. Can custom verification emails include custom headers?<a name="custom-verification-emails-faq-q9"></a>

Custom verification emails cannot include custom headers\.

### Q10\. Can I remove the text that appears at the bottom of custom verification emails?<a name="custom-verification-emails-faq-q10"></a>

The following text is automatically added to the end of every custom verification email and cannot be removed:

*If you did not request to verify this email address, please disregard this message\. If you have any concerns, please forward this message to the following [email address](mailto:ses-enforcement@amazon.com) along with your questions or comments\. *

The *email address* link in this text refers to ses\-enforcement@amazon\.com, an inbox that is actively monitored by the Amazon SES team\.

### Q11\. Are custom verification emails DKIM\-signed?<a name="custom-verification-emails-faq-q11"></a>

In order for verification emails to be DKIM\-signed, the email address that you specify in the `FromEmailAddress` attribute when you create the verification email template must be configured to generate a DKIM signature\. For more information about setting up DKIM for domains and email addresses, see [Authenticating Email with DKIM in Amazon SES](dkim.md)\.

### Q12\. Why don't the custom verification email template API operations appear in the SDK or CLI?<a name="custom-verification-emails-faq-q12"></a>

If you're unable to use the custom verification email template operations in an SDK or the AWS CLI, you may be using an older version of the SDK or CLI\. The custom verification email template operations are available in the following SDKs and CLIs: 

+ Version 1\.14\.6 or later of the AWS Command Line Interface

+ Version 3\.3\.205\.0 or later of the AWS SDK for \.NET

+ Version 1\.3\.20170531\.19 or later of the AWS SDK for C\+\+

+ Version 1\.12\.43 or later of the AWS SDK for Go

+ Version 1\.11\.245 or later of the AWS SDK for Java

+ Version 2\.166\.0 or later of the AWS SDK for JavaScript

+ Version 3\.45\.2 or later of the AWS SDK for PHP

+ Version 1\.5\.1 or later of the AWS SDK for Python \(Boto\)

+ Version 1\.5\.0 or later of the `aws-sdk-ses` gem in the AWS SDK for Ruby