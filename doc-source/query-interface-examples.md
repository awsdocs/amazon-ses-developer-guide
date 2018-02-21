# GET and POST Examples for Amazon SES<a name="query-interface-examples"></a>

The following are examples of GET and POST requests, using the Query API\.

## Example GET Request<a name="query-interface-get-examples"></a>

Here is an example of what a GET request might look like, including the calculated signature\. Notice that all of the parameters have been URL\-encoded\.

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

## Example POST Request<a name="query-interface-post-examples"></a>

Here is an example of what a POST request might look like, before calculating the signature\. Notice that all of the parameters have been URL\-encoded\.

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

Following is the complete POST request to *SendRawEmail*, with the `X-Amzn-Authorization` header\. None of the headers should be URL\-encoded\.

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