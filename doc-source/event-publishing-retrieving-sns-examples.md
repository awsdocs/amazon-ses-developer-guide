# Examples of Amazon SES Event Data Published to Amazon SNS<a name="event-publishing-retrieving-sns-examples"></a>

This section provides examples of each type of email sending event record that Amazon SES publishes to Amazon SNS\.

**Topics**
+ [Bounce Record](#event-publishing-retrieving-sns-bounce)
+ [Complaint Record](#event-publishing-retrieving-sns-complaint)
+ [Delivery Record](#event-publishing-retrieving-sns-delivery)
+ [Send Email Record](#event-publishing-retrieving-sns-send)
+ [Reject Event Record](#event-publishing-retrieving-sns-reject)
+ [Open Event Record](#event-publishing-retrieving-sns-open)
+ [Click Event Record](#event-publishing-retrieving-sns-click)
+ [Rendering Failure Event Record](#event-publishing-retrieving-sns-failure)

## Bounce Record<a name="event-publishing-retrieving-sns-bounce"></a>

The following is an example of a `bounce` event record that Amazon SES publishes to Amazon SNS\. 

```
 1. {
 2.   "notificationType":"Bounce",
 3.   "bounce":{
 4.     "bounceType":"Permanent",
 5.     "bounceSubType":"General",
 6.     "bouncedRecipients":[
 7.       {
 8.         "emailAddress":"recipient@example.com",
 9.         "action":"failed",
10.         "status":"5.1.1",
11.         "diagnosticCode":"smtp; 550 5.1.1 user unknown"
12.       }
13.     ],
14.     "timestamp":"2017-08-05T00:41:02.669Z",
15.     "feedbackId":"01000157c44f053b-61b59c11-9236-11e6-8f96-7be8aexample-000000",
16.     "reportingMTA":"dsn; mta.example.com"
17.   },
18.   "mail":{
19.     "timestamp":"2017-08-05T00:40:02.012Z",
20.     "source":"Sender Name <sender@example.com>",
21.     "sourceArn":"arn:aws:ses:us-east-1:123456789012:identity/sender@example.com",
22.     "sendingAccountId":"123456789012",
23.     "messageId":"EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
24.     "destination":[
25.       "recipient@example.com"
26.     ],
27.     "headersTruncated":false,
28.     "headers":[
29.       {
30.         "name":"From",
31.         "value":"Sender Name <sender@example.com>"
32.       },
33.       {
34.         "name":"To",
35.         "value":"recipient@example.com"
36.       },
37.       {
38.         "name":"Subject",
39.         "value":"Message sent from Amazon SES"
40.       },
41.       {
42.         "name":"MIME-Version",
43.         "value":"1.0"
44.       },
45.       {
46.         "name":"Content-Type",
47.         "value":"multipart/alternative; boundary=\"----=_Part_7307378_1629847660.1516840721503\""
48.       }
49.     ],
50.     "commonHeaders":{
51.       "from":[
52.         "Sender Name <sender@example.com>"
53.       ],
54.       "to":[
55.         "recipient@example.com"
56.       ],
57.       "messageId":"EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
58.       "subject":"Message sent from Amazon SES"
59.     },
60.     "tags":{
61.       "ses:configuration-set":[
62.         "ConfigSet"
63.       ],
64.       "ses:source-ip":[
65.         "192.0.2.0"
66.       ],
67.       "ses:from-domain":[
68.         "example.com"
69.       ],
70.       "ses:caller-identity":[
71.         "ses_user"
72.       ]
73.     }
74.   }
75. }
```

## Complaint Record<a name="event-publishing-retrieving-sns-complaint"></a>

The following is an example of a `complaint` event record that Amazon SES publishes to Amazon SNS\. 

```
 1. {
 2.   "notificationType":"Complaint",
 3.   "complaint": {
 4.     "complainedRecipients":[
 5.       {
 6.         "emailAddress":"recipient@example.com"
 7.       }
 8.     ],
 9.     "timestamp":"2017-08-05T00:41:02.669Z",
10.     "feedbackId":"01000157c44f053b-61b59c11-9236-11e6-8f96-7be8aexample-000000",
11.     "userAgent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.90 Safari/537.36",
12.     "complaintFeedbackType":"abuse",
13.     "arrivalDate":"2017-08-05T00:41:02.669Z"
14.   },
15.   "mail":{
16.     "timestamp":"2017-08-05T00:40:01.123Z",
17.     "source":"Sender Name <sender@example.com>",
18.     "sourceArn":"arn:aws:ses:us-east-1:123456789012:identity/sender@example.com",
19.     "sendingAccountId":"123456789012",
20.     "messageId":"EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
21.     "destination":[
22.       "recipient@example.com"
23.     ],
24.     "headersTruncated":false,
25.     "headers":[
26.       {
27.         "name":"From",
28.         "value":"Sender Name <sender@example.com>"
29.       },
30.       {
31.         "name":"To",
32.         "value":"recipient@example.com"
33.       },
34.       {
35.         "name":"Subject",
36.         "value":"Message sent from Amazon SES"
37.       },
38.       {
39.         "name":"MIME-Version","value":"1.0"
40.       },
41.       {
42.         "name":"Content-Type",
43.         "value":"multipart/alternative; boundary=\"----=_Part_7298998_679725522.1516840859643\""
44.       }
45.     ],
46.     "commonHeaders":{
47.       "from":[
48.         "Sender Name <sender@example.com>"
49.       ],
50.       "to":[
51.         "recipient@example.com"
52.       ],
53.       "messageId":"EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
54.       "subject":"Message sent from Amazon SES"
55.     },
56.     "tags":{
57.       "ses:configuration-set":[
58.         "ConfigSet"
59.       ],
60.       "ses:source-ip":[
61.         "192.0.2.0"
62.       ],
63.       "ses:from-domain":[
64.         "example.com"
65.       ],
66.       "ses:caller-identity":[
67.         "ses_user"
68.       ]
69.     }
70.   }
71. }
```

## Delivery Record<a name="event-publishing-retrieving-sns-delivery"></a>

The following is an example of a `delivery` event record that Amazon SES publishes to Amazon SNS\. 

```
 1. {
 2.   "notificationType": "Delivery",
 3.   "mail": {
 4.     "timestamp": "2016-10-19T23:20:52.240Z",
 5.     "source": "sender@example.com",
 6.     "sourceArn": "arn:aws:ses:us-east-1:123456789012:identity/sender@example.com",
 7.     "sendingAccountId": "123456789012",
 8.     "messageId": "EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
 9.     "destination": [
10.       "recipient@example.com"
11.     ],
12.     "headersTruncated": false,
13.     "headers": [
14.       {
15.         "name": "From",
16.         "value": "sender@example.com"
17.       },
18.       {
19.         "name": "To",
20.         "value": "recipient@example.com"
21.       },
22.       {
23.         "name": "Subject",
24.         "value": "Message sent from Amazon SES"
25.       },
26.       {
27.         "name": "MIME-Version",
28.         "value": "1.0"
29.       },
30.       {
31.         "name": "Content-Type",
32.         "value": "text/html; charset=UTF-8"
33.       },
34.       {
35.         "name": "Content-Transfer-Encoding",
36.         "value": "7bit"
37.       }
38.     ],
39.     "commonHeaders": {
40.       "from": [
41.         "sender@example.com"
42.       ],
43.       "to": [
44.         "recipient@example.com"
45.       ],
46.       "messageId": "EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
47.       "subject": "Message sent from Amazon SES"
48.     },
49.     "tags": {
50.       "ses:configuration-set": [
51.         "ConfigSet"
52.       ],
53.       "ses:source-ip": [
54.         "192.0.2.0"
55.       ],
56.       "ses:from-domain": [
57.         "example.com"
58.       ],
59.       "ses:caller-identity": [
60.         "ses_user"
61.       ],
62.       "ses:outgoing-ip": [
63.         "192.0.2.0"
64.       ],
65.       "myCustomTag1": [
66.         "myCustomTagValue1"
67.       ],
68.       "myCustomTag2": [
69.         "myCustomTagValue2"
70.       ]      
71.     }
72.   },
73.   "delivery": {
74.     "timestamp": "2016-10-19T23:21:04.133Z",
75.     "processingTimeMillis": 11893,
76.     "recipients": [
77.       "recipient@example.com"
78.     ],
79.     "smtpResponse": "250 2.6.0 Message received",
80.     "reportingMTA": "mta.example.com"
81.   }
82. }
```

## Send Email Record<a name="event-publishing-retrieving-sns-send"></a>

The following is an example of a `send` event record that Amazon SES publishes to Amazon SNS\. 

```
 1. {
 2.   "notificationType": "Send",
 3.   "mail": {
 4.     "timestamp": "2016-10-14T05:02:16.645Z",
 5.     "source": "sender@example.com",
 6.     "sourceArn": "arn:aws:ses:us-east-1:123456789012:identity/sender@example.com",
 7.     "sendingAccountId": "123456789012",
 8.     "messageId": "EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
 9.     "destination": [
10.       "recipient@example.com"
11.     ],
12.     "headersTruncated": false,
13.     "headers": [
14.       {
15.         "name": "From",
16.         "value": "sender@example.com"
17.       },
18.       {
19.         "name": "To",
20.         "value": "recipient@example.com"
21.       },
22.       {
23.         "name": "Subject",
24.         "value": "Message sent from Amazon SES"
25.       },
26.       {
27.         "name": "MIME-Version",
28.         "value": "1.0"
29.       },
30.       {
31.         "name": "Content-Type",
32.         "value": "multipart/mixed;  boundary=\"----=_Part_0_716996660.1476421336341\""
33.       },
34.       {
35.         "name": "X-SES-MESSAGE-TAGS",
36.         "value": "myCustomTag1=myCustomTagValue1, myCustomTag2=myCustomTagValue2"
37.       }
38.     ],
39.     "commonHeaders": {
40.       "from": [
41.         "sender@example.com"
42.       ],
43.       "to": [
44.         "recipient@example.com"
45.       ],
46.       "messageId": "EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
47.       "subject": "Message sent from Amazon SES"
48.     },
49.     "tags": {
50.       "ses:configuration-set": [
51.         "ConfigSet"
52.       ],
53.       "ses:source-ip": [
54.         "192.0.2.0"
55.       ],
56.       "ses:from-domain": [
57.         "example.com"
58.       ],      
59.       "ses:caller-identity": [
60.         "ses_user"
61.       ],
62.       "myCustomTag1": [
63.         "myCustomTagValue1"
64.       ],
65.       "myCustomTag2": [
66.         "myCustomTagValue2"
67.       ]      
68.     }
69.   },
70.   "send": {}
71. }
```

## Reject Event Record<a name="event-publishing-retrieving-sns-reject"></a>

The following is an example of a `reject` event record that Amazon SES publishes to Amazon SNS\. 

```
 1. {
 2.   "notificationType": "Reject",
 3.   "mail": {
 4.     "timestamp": "2016-10-14T17:38:15.211Z",
 5.     "source": "sender@example.com",
 6.     "sourceArn": "arn:aws:ses:us-east-1:123456789012:identity/sender@example.com",
 7.     "sendingAccountId": "123456789012",
 8.     "messageId": "EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
 9.     "destination": [
10.       "sender@example.com"
11.     ],
12.     "headersTruncated": false,
13.     "headers": [
14.       {
15.         "name": "From",
16.         "value": "sender@example.com"
17.       },
18.       {
19.         "name": "To",
20.         "value": "recipient@example.com"
21.       },      
22.       {
23.         "name": "Subject",
24.         "value": "Message sent from Amazon SES"
25.       },
26.       {
27.         "name": "MIME-Version",
28.         "value": "1.0"
29.       },      
30.       {
31.         "name": "Content-Type",
32.         "value": "multipart/mixed; boundary=\"qMm9M+Fa2AknHoGS\""
33.       },
34.       {
35.         "name": "X-SES-MESSAGE-TAGS",
36.         "value": "myCustomTag1=myCustomTagValue1, myCustomTag2=myCustomTagValue2"
37.       }  
38.     ],
39.     "commonHeaders": {
40.       "from": [
41.         "sender@example.com"
42.       ],
43.       "to": [
44.         "recipient@example.com"
45.       ],
46.       "messageId": "EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
47.       "subject": "Message sent from Amazon SES"
48.     },
49.     "tags": {
50.       "ses:configuration-set": [
51.         "ConfigSet"
52.       ],
53.       "ses:source-ip": [
54.         "192.0.2.0"
55.       ],
56.       "ses:from-domain": [
57.         "example.com"
58.       ],    
59.       "ses:caller-identity": [
60.         "ses_user"
61.       ],
62.       "myCustomTag1": [
63.         "myCustomTagValue1"
64.       ],
65.       "myCustomTag2": [
66.         "myCustomTagValue2"
67.       ]      
68.     }
69.   },
70.   "reject": {
71.     "reason": "Bad content"
72.   }
73. }
```

## Open Event Record<a name="event-publishing-retrieving-sns-open"></a>

The following is an example of an `open` event record that Amazon SES publishes to Amazon SNS\. 

```
 1. {
 2.   "notificationType": "Open",
 3.   "mail": {
 4.     "commonHeaders": {
 5.       "from": [
 6.         "sender@example.com"
 7.       ],
 8.       "messageId": "EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
 9.       "subject": "Message sent from Amazon SES",
10.       "to": [
11.         "recipient@example.com"
12.       ]
13.     },
14.     "destination": [
15.       "recipient@example.com"
16.     ],
17.     "headers": [
18.       {
19.         "name": "X-SES-CONFIGURATION-SET",
20.         "value": "ConfigSet"
21.       },
22.       {
23.         "name":"X-SES-MESSAGE-TAGS",
24.         "value":"myCustomTag1=myCustomValue1, myCustomTag2=myCustomValue2"
25.       },
26.       {
27.         "name": "From",
28.         "value": "sender@example.com"
29.       },
30.       {
31.         "name": "To",
32.         "value": "recipient@example.com"
33.       },
34.       {
35.         "name": "Subject",
36.         "value": "Message sent from Amazon SES"
37.       },
38.       {
39.         "name": "MIME-Version",
40.         "value": "1.0"
41.       },
42.       {
43.         "name": "Content-Type",
44.         "value": "multipart/alternative; boundary=\"XBoundary\""
45.       }
46.     ],
47.     "headersTruncated": false,
48.     "messageId": "EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
49.     "sendingAccountId": "123456789012",
50.     "source": "sender@example.com",
51.     "tags": {
52.       "myCustomTag1":[
53.         "myCustomValue1"
54.       ],
55.       "myCustomTag2":[
56.         "myCustomValue2"
57.       ],
58.       "ses:caller-identity": [
59.         "ses-user"
60.       ],
61.       "ses:configuration-set": [
62.         "ConfigSet"
63.       ],
64.       "ses:from-domain": [
65.         "example.com"
66.       ],
67.       "ses:source-ip": [
68.         "192.0.2.0"
69.       ]
70.     },
71.     "timestamp": "2017-08-09T21:59:49.927Z"
72.   },
73.   "open": {
74.     "ipAddress": "192.0.2.1",
75.     "timestamp": "2017-08-09T22:00:19.652Z",
76.     "userAgent": "Mozilla/5.0 (iPhone; CPU iPhone OS 10_3_3 like Mac OS X) AppleWebKit/603.3.8 (KHTML, like Gecko) Mobile/14G60"
77.   }
78. }
```

## Click Event Record<a name="event-publishing-retrieving-sns-click"></a>

The following is an example of a `click` event record that Amazon SES publishes to Amazon SNS\. 

```
 1. {
 2.   "notificationType": "Click",
 3.   "click": {
 4.     "ipAddress": "192.0.2.1",
 5.     "link": "http://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-email-smtp.html",
 6.     "linkTags": {
 7.       "samplekey0": [
 8.         "samplevalue0"
 9.       ],
10.       "samplekey1": [
11.         "samplevalue1"
12.       ]
13.     },
14.     "timestamp": "2017-08-09T23:51:25.570Z",
15.     "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.90 Safari/537.36"
16.   },
17.   "mail": {
18.     "commonHeaders": {
19.       "from": [
20.         "sender@example.com"
21.       ],
22.       "messageId": "EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
23.       "subject": "Message sent from Amazon SES",
24.       "to": [
25.         "recipient@example.com"
26.       ]
27.     },
28.     "destination": [
29.       "recipient@example.com"
30.     ],
31.     "headers": [
32.       {
33.         "name": "X-SES-CONFIGURATION-SET",
34.         "value": "ConfigSet"
35.       },
36.       {
37.         "name":"X-SES-MESSAGE-TAGS",
38.         "value":"myCustomTag1=myCustomValue1, myCustomTag2=myCustomValue2"
39.       },
40.       {
41.         "name": "From",
42.         "value": "sender@example.com"
43.       },
44.       {
45.         "name": "To",
46.         "value": "recipient@example.com"
47.       },
48.       {
49.         "name": "Subject",
50.         "value": "Message sent from Amazon SES"
51.       },
52.       {
53.         "name": "MIME-Version",
54.         "value": "1.0"
55.       },
56.       {
57.         "name": "Content-Type",
58.         "value": "multipart/alternative; boundary=\"XBoundary\""
59.       },
60.       {
61.         "name": "Message-ID",
62.         "value": "EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000"
63.       }
64.     ],
65.     "headersTruncated": false,
66.     "messageId": "EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
67.     "sendingAccountId": "123456789012",
68.     "source": "sender@example.com",
69.     "tags": {
70.       "myCustomTag1":[
71.         "myCustomValue1"
72.       ],
73.       "myCustomTag2":[
74.         "myCustomValue2"
75.       ],
76.       "ses:caller-identity": [
77.         "ses_user"
78.       ],
79.       "ses:configuration-set": [
80.         "ConfigSet"
81.       ],
82.       "ses:from-domain": [
83.         "example.com"
84.       ],
85.       "ses:source-ip": [
86.         "192.0.2.0"
87.       ]
88.     },
89.     "timestamp": "2017-08-09T23:50:05.795Z"
90.   }
91. }
```

## Rendering Failure Event Record<a name="event-publishing-retrieving-sns-failure"></a>

The following is an example of a `Rendering Failure` event record that Amazon SES publishes to Amazon SNS\. 

```
 1. {
 2.   "notificationType":"Rendering Failure",
 3.   "mail":{
 4.     "timestamp":"2018-01-22T18:43:06.197Z",
 5.     "source":"sender@example.com",
 6.     "sourceArn":"arn:aws:ses:us-east-1:123456789012:identity/sender@example.com",
 7.     "sendingAccountId":"123456789012",
 8.     "messageId":"EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
 9.     "destination":[
10.       "recipient@example.com"
11.     ],
12.     "headersTruncated":false,
13.     "tags":{
14.       "ses:configuration-set":[
15.         "ConfigSet"
16.       ]
17.     }
18.   },
19.   "failure":{
20.     "errorMessage":"Attribute 'attributeName' is not present in the rendering data.",
21.     "templateName":"MyTemplate"
22.   }
23. }
```