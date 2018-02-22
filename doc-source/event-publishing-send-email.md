# Step 3: Send Email Using Amazon SES Event Publishing<a name="event-publishing-send-email"></a>

After you [create a configuration set](event-publishing-create-configuration-set.md) and [add an event destination](event-publishing-add-event-destination.md), the last step to event publishing is to send your emails\.

To publish events associated with an email, you must provide the name of the configuration set to associate with the email\. Optionally, you can provide message tags to categorize the email\.

You provide this information to Amazon SES as either parameters to the email sending API, Amazon SES\-specific email headers, or custom headers in your MIME message\. The method you choose depends on which email sending interface you use, as shown in the following table\.


****  

| Email Sending Interface | Ways to Publish Events | 
| --- | --- | 
|  `SendEmail`  |  API parameters  | 
|  `SendRawEmail`  |  API parameters, Amazon SES\-specific email headers, or custom MIME headers   If you specify message tags using both headers and API parameters, Amazon SES uses only the message tags provided by the API parameters\. Amazon SES does not join message tags specified by API parameters and headers\.    | 
|  SMTP interface  |  Amazon SES\-specific email headers  | 

The following sections describe how to specify the configuration set and message tags using headers and using API parameters\.

+ [Using Amazon SES API Parameters](#event-publishing-using-ses-parameters)

+ [Using Amazon SES\-Specific Email Headers](#event-publishing-using-ses-headers)

+ [Using Custom Email Headers](#event-publishing-using-custom-headers)

Additionally, this guide contains several code examples that demonstrate how to send email programmatically using Amazon SES\. Each of these code examples includes a method of passing a configuration set when sending an email\. For more information, see [Amazon SES Code Examples](samplecodeindex.md)\.

## Using Amazon SES API Parameters<a name="event-publishing-using-ses-parameters"></a>

To use `[SendEmail](http://docs.aws.amazon.com/ses/latest/APIReference/API_SendEmail.html)` or `[SendRawEmail](http://docs.aws.amazon.com/ses/latest/APIReference/API_SendRawEmail.html)` with event publishing, you specify the configuration set and the message tags by passing data structures called `[ConfigurationSet](http://docs.aws.amazon.com/ses/latest/APIReference/API_ConfigurationSet.html)` and `[MessageTag](http://docs.aws.amazon.com/ses/latest/APIReference/API_MessageTag.html)` to the API call\.

For more information about using the Amazon SES API, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/)\.

## Using Amazon SES\-Specific Email Headers<a name="event-publishing-using-ses-headers"></a>

When you use `SendRawEmail` or the SMTP interface, you can specify the configuration set and the message tags by adding Amazon SES\-specific headers to the email\. Amazon SES removes the headers before sending the email\. The following table shows the names of the headers to use\. 


| Event Publishing Information | Header | 
| --- | --- | 
|  Configuration set  |  `X-SES-CONFIGURATION-SET`  | 
|  Message tags  |  `X-SES-MESSAGE-TAGS`  | 

The following example shows how the headers might look in a raw email that you submit to Amazon SES\.

```
 1. X-SES-MESSAGE-TAGS: tagName1=tagValue1, tagName2=tagValue2
 2. X-SES-CONFIGURATION-SET: myConfigurationSet
 3. From: sender@example.com
 4. To: recipient@example.com
 5. Subject: Subject
 6. Content-Type: multipart/alternative;
 7. 	boundary="----=_boundary"
 8. 
 9. ------=_boundary
10. Content-Type: text/plain; charset=UTF-8
11. Content-Transfer-Encoding: 7bit
12. 
13. body
14. ------=_boundary
15. Content-Type: text/html; charset=UTF-8
16. Content-Transfer-Encoding: 7bit
17. 
18. body
19. ------=_boundary--
```

## Using Custom Email Headers<a name="event-publishing-using-custom-headers"></a>

Although you must specify the configuration set name using the Amazon SES\-specific header `X-SES-CONFIGURATION-SET`, you can specify the message tags by using your own MIME headers\.

**Note**  
Header names and values that you use for Amazon SES event publishing must be in ASCII\. If you specify a non\-ASCII header name or value for Amazon SES event publishing, the email sending call will still succeed, but the event metrics will not be emitted to Amazon CloudWatch\.