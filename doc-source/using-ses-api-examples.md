# GET and POST examples for the Amazon SES API<a name="using-ses-api-examples"></a>

This section contains examples of requests that you can issue against the Amazon SES API\.

## Example GET request<a name="using-ses-api-examples-get"></a>

The following code is an example of a GET request\. It includes a calculated signature\. Notice that all of the parameters in the request are URL\-encoded\.

```
1. https://email.us-west-2.amazonaws.com/
2. ?Action=SendEmail
3. &Source=user%40example.com
4. &Destination.ToAddresses.member.1=allan%40example.com
5. &Message.Subject.Data=This%20is%20the%20subject%20line.
6. &Message.Body.Text.Data=Hello.%20I%20hope%20you%20are%20having%20a%20good%20day.
7. &AWSAccessKeyId=AKIAIOSFODNN7EXAMPLE
8. &Signature=RhU864jFu893mg7g9N9j9nr6h7EXAMPLE
9. &Algorithm=HMACSHA256
```

## Example POST request<a name="using-ses-api-examples-post"></a>

The following code is an example of a POST request\. It includes a calculated signature\. Notice that all of the parameters in the request are URL\-encoded\.

```
1. POST / HTTP/1.1
2. Host: email.us-west-2.amazonaws.com
3. Content-Type: application/x-www-form-urlencoded
4. Date: Tue, 25 May 2010 21:20:27 +0000
5. Content-Length: 174
6. 
7. Action=SendRawEmail
8. &Destinations.member.1=allan%40example.com
9. &RawMessage.Data=RnJvbTp1c2VyQGV4YW1wbGUuY29tDQpTdWJqZWN0OiBUZXN0DQoNCk1lc3 ...
```

The value for `RawMessage.Data` is a base64\-encoded representation of the following text\.

```
1. From:user@example.com
2. Subject: Test
3. 
4. Message sent using SendRawEmail.
```

The following is a complete POST request to the `SendRawEmail` operator\. The request includes the `X-Amzn-Authorization` header\. None of the headers are URL\-encoded\.

```
 1. POST / HTTP/1.1
 2. Host: email.us-west-2.amazonaws.com
 3. Content-Type: application/x-www-form-urlencoded
 4. Date: Tue, 25 May 2010 21:20:27 +0000
 5. Content-Length: 174
 6. X-Amzn-Authorization: AWS3-HTTPS AWSAccessKeyId=AKIAIOSFODNN7EXAMPLE,Algorithm=HMACSHA256,Signature=lBP67vCvGl ...
 7. 
 8. Action=SendRawEmail
 9. &Destinations.member.1=allan%40example.com
10. &RawMessage.Data=RnJvbTp1c2VyQGV4YW1wbGUuY29tDQpTdWJqZWN0OiBUZXN0DQoNCk1lc3 ...
```