# Sending Personalized Email Using the Amazon SES API<a name="send-personalized-email-api"></a>

You can use the `CreateTemplate` API operation to create email templates\. These templates include a subject line, and the text and HTML parts of the email body\. The subject and body sections may also contain unique values that are personalized for each recipient\.

There are a few limits and other considerations when using these features:
+ You can create up to 10,000 email templates per Amazon SES account\.
+ Each template can be up to 500KB in size, including both the text and HTML parts\.
+ You can include an unlimited number of replacement variables in each template\.
+ You can send email to up to 50 destinations in each call to the `SendBulkTemplatedEmail` operation\. A destination includes a list of recipients, as well as CC and BCC recipients\. Note that the number of destinations you can contact in a single call to the API may be limited by your account's maximum sending rate\. For more information, see [Managing Your Amazon SES Sending Quotas](manage-sending-quotas.md)\.

This section includes procedures for creating email templates and for sending personalized emails\.

## Part 1: Set up Rendering Failure Event Notifications<a name="send-personalized-email-set-up-notifications"></a>

 If you send an email that contains invalid personalization content, Amazon SES might accept the message, but won't be able to deliver it\. For this reason, if you plan to send personalized email, you should configure Amazon SES to send Rendering Failure event notifications through Amazon SNS\. When you receive a Rendering Failure event notification, you can identify which message contained the invalid content, fix the issues, and send the message again\.

The procedure in this section is optional, but highly recommended\.

**To configure Rendering Failure event notifications**

1. Create an Amazon SNS topic\. For procedures, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html) in the *Amazon Simple Notification Service Developer Guide*\.

1. Subscribe to the Amazon SNS topic\. For example, if you want to receive Rendering Failure notifications by email, subscribe an email endpoint \(that is, your email address\) to the topic\.

   For procedures, see [Subscribe to a Topic](https://docs.aws.amazon.com/sns/latest/dg/SubscribeTopic.html) in the *Amazon Simple Notification Service Developer Guide*\.

1. Complete the procedures in [Set up an Amazon SNS event destination for event publishing](event-publishing-add-event-destination-sns.md) to set up your configuration sets to publish Rendering Failure events to your Amazon SNS topic\.

## Part 2: Create an Email Template<a name="send-personalized-email-create-template"></a>

The instructions in this section assume that you are using the AWS CLI, and that you configured it to interact with your AWS account\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To create the template**

1. In a text editor, create a new file\. Paste the following code into the file\.

   ```
   1. {
   2.   "Template": {
   3.     "TemplateName": "MyTemplate",
   4.     "SubjectPart": "Greetings, {{name}}!",
   5.     "HtmlPart": "<h1>Hello {{name}},</h1><p>Your favorite animal is {{favoriteanimal}}.</p>",
   6.     "TextPart": "Dear {{name}},\r\nYour favorite animal is {{favoriteanimal}}."
   7.   }
   8. }
   ```

   This code contains the following properties:
   + **TemplateName** – The name of the template\. When you send the email, you refer to this name\.
   + **SubjectPart** – The subject line of the email\. This property may contain replacement tags\. These tags use the following format: `{{tagname}}`\. When you send the email, you can specify a value for `tagname` for each destination\.

     The preceding example includes two tags: `{{name}}` and `{{favoriteanimal}}`\.
   + **HtmlPart** – The HTML body of the email\. This property may contain replacement tags\.
   + **TextPart** – The text body of the email\. Recipients whose email clients don't display HTML email see this version of the email\. This property may contain replacement tags\.

1. Customize the preceding example to fit your needs, and then save the file as `mytemplate.json`\.

1. At the command line, type the following command to create a new template using the `CreateTemplate` API operation: aws ses create\-template \-\-cli\-input\-json file://mytemplate\.json

## Part 3: Sending the Personalized Email<a name="send-personalized-email-api-operations"></a>

After you create an email template, you can use it to send email\. There are two API operations that you can use to send emails using templates: `SendTemplatedEmail`, and `SendBulkTemplatedEmail`\. The `SendTemplatedEmail` operation is useful for sending a customized email to a single destination \(a collection of "To," "CC," and "BCC" recipients who will receive the same email\)\. The `SendBulkTemplatedEmail` operation is useful for sending unique emails to multiple destinations in a single call to the Amazon SES API\. This section provides examples of how to use the AWS CLI to send email using both of these operations\.

### Sending Templated Email to a Single Destination<a name="send-templated-email-single-destination"></a>

You can use the `SendTemplatedEmail` operation to send an email to a single destination\. All of the recipients in the `Destination` object will receive the same email\.

**To send a templated email to a single destination**

1. In a text editor, create a new file\. Paste the following code into the file\.

   ```
    1. {
    2.   "Source":"Mary Major <mary.major@example.com>",
    3.   "Template": "MyTemplate",
    4.   "ConfigurationSetName": "ConfigSet",
    5.   "Destination": {
    6.     "ToAddresses": [ "alejandro.rosalez@example.com"
    7.     ]
    8.   },
    9.   "TemplateData": "{ \"name\":\"Alejandro\", \"favoriteanimal\": \"alligator\" }"
   10. }
   ```

   This code contains the following properties:
   + **Source** – The email address of the sender\.
   + **Template** – The name of the template to apply to the email\.
   + **ConfigurationSetName** – The name of the configuration set to use when sending the email\.
**Note**  
We recommend that you use a configuration set that is configured to publish Rendering Failure events to Amazon SNS\. For more information, see [Part 1: Set up Rendering Failure Event Notifications](#send-personalized-email-set-up-notifications)\.
   + **Destination** – The recipient addresses\. You can include multiple "To," "CC," and "BCC" addresses\. When you use the `SendTemplatedEmail` operation, all recipients receive the same email\.
   + **TemplateData** – An escaped JSON string that contains key\-value pairs\. The keys correspond to the variables in the template \(for example, `{{name}}`\)\. The values represent the content that replaces the variables in the email\.

1. Change the values in the code above to meet your needs, and then save the file as `myemail.json`\.

1. At the command line, type the following command to send the email: aws ses send\-templated\-email \-\-cli\-input\-json file://myemail\.json

### Sending Templated Email to Multiple Destinations<a name="send-templated-email-multiple-destinations"></a>

You can use the `SendBulkTemplatedEmail` operation to send an email to several destinations in a single call to the API\. Amazon SES sends a unique email to the recipient or recipients in each `Destination` object\.

**To send a templated email to multiple destinations**

1. In a text editor, create a new file\. Paste the following code into the file\.

   ```
    1. {
    2.   "Source":"Mary Major <mary.major@example.com>",
    3.   "Template":"MyTemplate",
    4.   "ConfigurationSetName": "ConfigSet",
    5.   "Destinations":[
    6.     {
    7.       "Destination":{
    8.         "ToAddresses":[
    9.           "anaya.iyengar@example.com"
   10.         ]
   11.       },
   12.       "ReplacementTemplateData":"{ \"name\":\"Anaya\", \"favoriteanimal\":\"angelfish\" }"
   13.     },
   14.     {
   15.       "Destination":{ 
   16.         "ToAddresses":[
   17.           "liu.jie@example.com"
   18.         ]
   19.       },
   20.       "ReplacementTemplateData":"{ \"name\":\"Liu\", \"favoriteanimal\":\"lion\" }"
   21.     },
   22.     {
   23.       "Destination":{
   24.         "ToAddresses":[
   25.           "shirley.rodriguez@example.com"
   26.         ]
   27.       },
   28.       "ReplacementTemplateData":"{ \"name\":\"Shirley\", \"favoriteanimal\":\"shark\" }"
   29.     },
   30.     {
   31.       "Destination":{
   32.         "ToAddresses":[
   33.           "richard.roe@example.com"
   34.         ]
   35.       },
   36.       "ReplacementTemplateData":"{}"
   37.     }
   38.   ],
   39.   "DefaultTemplateData":"{ \"name\":\"friend\", \"favoriteanimal\":\"unknown\" }"
   40. }
   ```

   This code contains the following properties:
   + **Source** – The email address of the sender\.
   + **Template** – The name of the template to apply to the email\.
   + **ConfigurationSetName** – The name of the configuration set to use when sending the email\.
**Note**  
We recommend that you use a configuration set that is configured to publish Rendering Failure events to Amazon SNS\. For more information, see [Part 1: Set up Rendering Failure Event Notifications](#send-personalized-email-set-up-notifications)\.
   + **Destinations** – An array that contains one or more Destinations\.
     + **Destination** – The recipient addresses\. You can include multiple "To," "CC," and "BCC" addresses\. When you use the `SendBulkTemplatedEmail` operation, all recipients within the same `Destination` object receive the same email\.
     + **ReplacementTemplateData** – A JSON object that contains key\-value pairs\. The keys correspond to the variables in the template \(for example, `{{name}}`\)\. The values represent the content that replaces the variables in the email\.
   + **DefaultTemplateData** – A JSON object that contains key\-value pairs\. The keys correspond to the variables in the template \(for example, `{{name}}`\)\. The values represent the content that replaces the variables in the email\. This object contains fallback data\. If a `Destination` object contains an empty JSON object in the `ReplacementTemplateData` property, the values in the `DefaultTemplateData` property are used\.

1. Change the values in the code above to meet your needs, and then save the file as `mybulkemail.json`\.

1. At the command line, type the following command to send the bulk email: aws ses send\-bulk\-templated\-email \-\-cli\-input\-json file://mybulkemail\.json