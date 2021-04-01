# Amazon SES API requests<a name="using-ses-api-requests"></a>

You can call the Amazon SES API directly by making requests to an Amazon SES API endpoint\. Requests to the Amazon SES API are simple HTTPS requests that use the GET or POST method\. API requests have to contain an `Action` parameter to indicate the action to be performed\.

**Important**  
Amazon SES doesn't support HTTP requests\. You have to use HTTPS instead\.

## Structure of a GET request<a name="using-ses-api-requests-get"></a>

This guide presents the Amazon SES GET requests as URLs\. Each URL consists of the following:
+ **Endpoint—**The resource the request is acting on\. For a list of endpoint URLs for the AWS Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.
+ **Action—**The action you want to perform on the endpoint, such as sending a message\.
+ **Parameters—**Any request parameters\.

The following is an example GET request to send a message using the Amazon SES endpoint in the US West \(Oregon\) region\.

```
1. https://email.us-west-2.amazonaws.com?Action=SendEmail&Source=user%40example.com&Destination.ToAddresses.member.1=allan%40example.com&Message.Subject.Data=This%20is%20the%20subject%20line.&Message.Body.Text.Data=Hello.%20I%20hope%20you%20are%20having%20a%20good%20day.
```

**Important**  
Because the GET requests are URLs, you must URL\-encode the parameter values\. For example, in the preceding example request, the value for the `Source` parameter is actually `user@example.com`\. However, the "@" character is not allowed in URLs, so each "@" is URL\-encoded as "`%40`"\.

To make the GET examples easier to read, this guide presents them in the following parsed format\.

```
1. https://email.us-west-2.amazonaws.com
2. ?Action=SendEmail
3. &Source=user%40example.com
4. &Destination.ToAddresses.member.1=allan%40example.com
5. &Message.Subject.Data=This%20is%20the%20subject%20line.
6. &Message.Body.Text.Data=Hello.%20I%20hope%20you%20are%20having%20a%20good%20day.
```

The first line represents the *endpoint* of the request\. After the endpoint is a question mark \(?\), which separates the endpoint from the parameters\. Each parameter is separated by an ampersand \(&\)\.

The `Action` parameter indicates the action to perform\. For a complete list of actions, and the parameters used with each action, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\.

Some operations take lists of parameters\. For example, when you send an email to multiple recipients, you can provide a list of email addresses\. You specify this type of list with `param.n` notation, where values of *n* are integers starting from 1\. For example, you would specify the first "To:" address using `Destination.ToAddresses.1`, the second with `Destination.ToAddresses.2`, etc\.

In Amazon SES, spaces are not allowed in any of the parameter values\. In this guide, any example request parameter value that includes spaces is displayed in one of two different ways:
+ URL\-encoded \(as `%20`\)\.
+ Represented by a plus sign \("\+"\)\. Within a request, a plus sign is reserved as a shorthand notation for a space\. \(If you want to include a literal, uninterpreted plus sign in any parameter, you must URL\-encode it as `%2B`\.\)

**Note**  
Every request must be accompanied by an `X-Amzn-Authorization` HTTP header\. For more information, see [Authenticating requests to the Amazon SES API](using-ses-api-authentication.md)\.

## Structure of a POST request<a name="using-ses-api-requests-post"></a>

Amazon SES also accepts POST requests\. With a POST request, you send the parameters as a form in the HTTP request body as described in the following procedure\.

**To create a POST request**

1. Assemble the parameter names and values into a form\.

   Put the parameters and values together as you would for a GET request \(with an ampersand separating each name\-value pair\)\. The following example shows a `SendEmail` request with the line breaks we use in this guide to make the information easier to read\.

   ```
   1. Action=SendEmail
   2. &Source=user@example.com
   3. &Destination.ToAddresses.member.1=allan@example.com
   4. &Message.Subject.Data=This is the subject line.
   5. &Message.Body.Text.Data=Hello. I hope you are having a good day.
   ```

   

1. Form\-URL\-encode the form according to the Form Submission section of the HTML specification\.

   For more information, see [http://www\.w3\.org/MarkUp/html\-spec/html\-spec\_toc\.html\#SEC8\.2\.1](http://www.w3.org/MarkUp/html-spec/html-spec_toc.html#SEC8.2.1)\.

   ```
   1. Action=SendEmail
   2. &Source=user%40example.com
   3. &Destination.ToAddresses.member.1=allan%40example.com
   4. &Message.Subject.Data=This%20is%20the%20subject%20line.
   5. &Message.Body.Text.Data=Hello.%20I%20hope%20you%20are%20having%20a%20good%20day.
   ```

1. Provide the resulting form as the body of the POST request\.

1. Include the following HTTP headers in the request:
   + `Content-Type`, with the value set to `application/x-www-form-urlencoded`
   + `Content-Length`
   + `Date`
   + `Authorization` \(For more information, see [Authenticating requests to the Amazon SES API](using-ses-api-authentication.md)\.\) 

1. Send the completed request\.

   ```
    1. POST / HTTP/1.1
    2. Date: Thu, 26 May 2011 06:49:50 GMT
    3. Host: email.us-west-2.amazonaws.com
    4. Content-Type: application/x-www-form-urlencoded
    5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE,SignedHeaders=Date;x-amz-date,Signature=9d63c3b5b7623d1fa3dc7fd1547313b9546c6d0fbbb6773a420613b7EXAMPLE
    6. Content-Length: 230
    7. 
    8. Action=SendEmail
    9. &Source=user%40example.com
   10. &Destination.ToAddresses.member.1=allan%40example.com
   11. &Message.Subject.Data=This%20is%20the%20subject%20line.
   12. &Message.Body.Text.Data=Hello.%20I%20hope%20you%20are%20having%20a%20good%20day.
   ```

**Note**  
Your HTTP client typically adds other items to the HTTP request as required by the version of HTTP that the client uses\. We don't include those additional items in the examples in this guide\.
