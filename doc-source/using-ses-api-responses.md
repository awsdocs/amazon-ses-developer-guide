# Amazon SES API Responses<a name="using-ses-api-responses"></a>

 In response to an API request, Amazon SES returns an XML data structure that contains the results of the request\.

Every Amazon SES response includes a request ID in a `RequestId` element\. The value is a unique string that AWS assigns\. If you ever have issues with a particular request, AWS will ask for the request ID to help troubleshoot the issue\.

Successful Amazon SES responses also include one or more message IDs\. You can think of a message ID as a receipt for an email message that Amazon SES sends\. If a message is rejected or bounced, the message ID will appear in any complaint or bounce notifications that you receive; you can then use the message ID to identify any problematic email messages that you have sent, and take corrective action\.

## Structure of a Successful Response<a name="using-ses-api-responses-structure"></a>

If the request succeeded, the main response element is named after the action, but with "Response" appended\. For example, `SendEmailResponse` is the response element returned for a successful `SendEmail` request\. This element contains the following child elements:
+ `ResponseMetadata`, which contains the `RequestId` child element\.
+ An optional element containing action\-specific results\. For example, the `SendEmailResponse` element includes an element called `SendEmailResult`\.

The XML schema describes the XML response message for each Amazon SES action\.

The following is an example of a successful response\.

```
1. <SendEmailResponse xmlns="https://email.amazonaws.com/doc/2010-03-31/">
2.   <SendEmailResult>
3.     <MessageId>000001271b15238a-fd3ae762-2563-11df-8cd4-6d4e828a9ae8-000000</MessageId>
4.   </SendEmailResult>
5.   <ResponseMetadata>
6.     <RequestId>fd3ae762-2563-11df-8cd4-6d4e828a9ae8</RequestId>
7.   </ResponseMetadata>
8. </SendEmailResponse>
```

## Structure of an Amazon SES API Error Response<a name="using-ses-api-responses-structure-error"></a>

If a request is unsuccessful, the main response element is called `ErrorResponse` regardless of the action that was called\. This element contains an `Error` element and a `RequestId` element\. Each `Error` includes:
+ A `Type` element that identifies whether the error was a receiver or sender error
+ A `Code` element that identifies the type of error that occurred
+ A `Message` element that describes the error condition in a human\-readable form
+ A `Detail` element that might give additional details about the error or might be empty

The following is an example of an error response\.

```
 1. <ErrorResponse>
 2.    <Error>
 3.       <Type>
 4.          Sender
 5.       </Type>
 6.       <Code>
 7.          ValidationError
 8.       </Code>
 9.       <Message>
10.          Value null at 'message.subject' failed to satisfy constraint: Member must not be null
11.       </Message>
12.    </Error>
13.    <RequestId>
14.       42d59b56-7407-4c4a-be0f-4c88daeea257
15.    </RequestId>
16. </ErrorResponse>
```