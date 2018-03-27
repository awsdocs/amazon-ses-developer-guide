# Amazon SNS Notification Examples for Amazon SES<a name="notification-examples"></a>

The following sections provide examples of the three types of notifications:
+ For bounce notification examples, see [Amazon SNS Bounce Notification Examples](#notification-examples-bounce)\.
+ For complaint notification examples, see [Amazon SNS Complaint Notification Examples](#notification-examples-complaint)\.
+ For delivery notification examples, see [Amazon SNS Delivery Notification Example](#notification-examples-delivery)\.

## Amazon SNS Bounce Notification Examples<a name="notification-examples-bounce"></a>

This section contains examples of bounce notifications with and without a Delivery Status Notification \(DSN\) provided by the email receiver that sent the feedback\.

### Bounce Notification With a DSN<a name="notification-examples-bounce-with-dsn"></a>

The following is an example of a bounce notification that contains a DSN and the original email headers\. When bounce notifications are not configured to include the original email headers, the `mail` object within the notifications does not include the `headersTruncated`, `headers`, and `commonHeaders` fields\.

```
   {
       "notificationType":"Bounce",
       "bounce":{
          "bounceType":"Permanent",
          "reportingMTA":"dns; email.example.com",
          "bouncedRecipients":[
             {
                "emailAddress":"jane@example.com",
                "status":"5.1.1",
                "action":"failed",
                "diagnosticCode":"smtp; 550 5.1.1 <jane@example.com>... User"
             }
          ],
          "bounceSubType":"General",
          "timestamp":"2016-01-27T14:59:38.237Z",
          "feedbackId":"00000138111222aa-33322211-cccc-cccc-cccc-ddddaaaa068a-000000",
          "remoteMtaIp":"127.0.2.0"
       },
       "mail":{
          "timestamp":"2016-01-27T14:59:38.237Z",
          "source":"john@example.com",
          "sourceArn": "arn:aws:ses:us-west-2:888888888888:identity/example.com",
          "sourceIp": "127.0.3.0",
          "sendingAccountId":"123456789012",
          "messageId":"00000138111222aa-33322211-cccc-cccc-cccc-ddddaaaa0680-000000",
          "destination":[
            "jane@example.com",
            "mary@example.com",
            "richard@example.com"],
          "headersTruncated":false,
          "headers":[ 
           { 
             "name":"From",
             "value":"\"John Doe\" <john@example.com>"
           },
           { 
             "name":"To",
             "value":"\"Jane Doe\" <jane@example.com>, \"Mary Doe\" <mary@example.com>, \"Richard Doe\" <richard@example.com>"
           },
           { 
             "name":"Message-ID",
             "value":"custom-message-ID"
           },
           { 
             "name":"Subject",
             "value":"Hello"
           },
           { 
             "name":"Content-Type",
             "value":"text/plain; charset=\"UTF-8\""
           },
           { 
             "name":"Content-Transfer-Encoding",
             "value":"base64"
           },
           { 
             "name":"Date",
             "value":"Wed, 27 Jan 2016 14:05:45 +0000"
           }
          ],
          "commonHeaders":{ 
             "from":[ 
                "John Doe <john@example.com>"
             ],
             "date":"Wed, 27 Jan 2016 14:05:45 +0000",
             "to":[ 
                "Jane Doe <jane@example.com>, Mary Doe <mary@example.com>, Richard Doe <richard@example.com>"
             ],
             "messageId":"custom-message-ID",
             "subject":"Hello"
           }
        }
    }
```

### Bounce Notification Without a DSN<a name="notification-examples-bounce-no-dsn"></a>

The following is an example of a bounce notification that includes the original email headers but does not include a DSN\. When bounce notifications are not configured to include the original email headers, the `mail` object within the notifications does not include the `headersTruncated`, `headers`, and `commonHeaders` fields\.

```
   {
      "notificationType":"Bounce",
      "bounce":{
         "bounceType":"Permanent",
         "bounceSubType": "General",
         "bouncedRecipients":[
            {
               "emailAddress":"jane@example.com"
            },
            {
               "emailAddress":"richard@example.com"
            }
         ],
         "timestamp":"2016-01-27T14:59:38.237Z",
         "feedbackId":"00000137860315fd-869464a4-8680-4114-98d3-716fe35851f9-000000",
         "remoteMtaIp":"127.0.2.0"
      },
      "mail":{
         "timestamp":"2016-01-27T14:59:38.237Z",
         "messageId":"00000137860315fd-34208509-5b74-41f3-95c5-22c1edc3c924-000000",
         "source":"john@example.com",
         "sourceArn": "arn:aws:ses:us-west-2:888888888888:identity/example.com",
         "sourceIp": "127.0.3.0",
         "sendingAccountId":"123456789012",
         "destination":[
            "jane@example.com",
            "mary@example.com",
            "richard@example.com"
         ],
        "headersTruncated":false,
        "headers":[ 
         { 
            "name":"From",
            "value":"\"John Doe\" <john@example.com>"
         },
         { 
            "name":"To",
            "value":"\"Jane Doe\" <jane@example.com>, \"Mary Doe\" <mary@example.com>, \"Richard Doe\" <richard@example.com>"
         },
         { 
            "name":"Message-ID",
            "value":"custom-message-ID"
         },
         { 
            "name":"Subject",
            "value":"Hello"
         },
         { 
            "name":"Content-Type",
            "value":"text/plain; charset=\"UTF-8\""
         },
         { 
            "name":"Content-Transfer-Encoding",
            "value":"base64"
         },
         { 
            "name":"Date",
            "value":"Wed, 27 Jan 2016 14:05:45 +0000"
          }
         ],
         "commonHeaders":{ 
           "from":[ 
              "John Doe <john@example.com>"
           ],
           "date":"Wed, 27 Jan 2016 14:05:45 +0000",
           "to":[ 
              "Jane Doe <jane@example.com>, Mary Doe <mary@example.com>, Richard Doe <richard@example.com>"
           ],
           "messageId":"custom-message-ID",
           "subject":"Hello"
         }
      }
  }
```

## Amazon SNS Complaint Notification Examples<a name="notification-examples-complaint"></a>

This section contains examples of complaint notifications with and without a feedback report provided by the email receiver that sent the feedback\.

### Complaint Notification With a Feedback Report<a name="notification-examples-complaint-with-feedback"></a>

The following is an example of a complaint notification that contains a feedback report and the original email headers\. When complaint notifications are not configured to include the original email headers, the `mail` object within the notifications does not include the `headersTruncated`, `headers`, and `commonHeaders` fields\.

```
   {
      "notificationType":"Complaint",
      "complaint":{
         "userAgent":"AnyCompany Feedback Loop (V0.01)",
         "complainedRecipients":[
            {
               "emailAddress":"richard@example.com"
            }
         ],
         "complaintFeedbackType":"abuse",
         "arrivalDate":"2016-01-27T14:59:38.237Z",
         "timestamp":"2016-01-27T14:59:38.237Z",
         "feedbackId":"000001378603177f-18c07c78-fa81-4a58-9dd1-fedc3cb8f49a-000000"
      },
      "mail":{
         "timestamp":"2016-01-27T14:59:38.237Z",
         "messageId":"000001378603177f-7a5433e7-8edb-42ae-af10-f0181f34d6ee-000000",
         "source":"john@example.com",
         "sourceArn": "arn:aws:ses:us-west-2:888888888888:identity/example.com",
         "sourceIp": "127.0.3.0",
         "sendingAccountId":"123456789012",
         "destination":[
            "jane@example.com",
            "mary@example.com",
            "richard@example.com"
         ], 
          "headersTruncated":false,
          "headers":[ 
           { 
             "name":"From",
             "value":"\"John Doe\" <john@example.com>"
           },
           { 
             "name":"To",
             "value":"\"Jane Doe\" <jane@example.com>, \"Mary Doe\" <mary@example.com>, \"Richard Doe\" <richard@example.com>"
           },
           { 
             "name":"Message-ID",
             "value":"custom-message-ID"
           },
           { 
             "name":"Subject",
             "value":"Hello"
           },
           { 
             "name":"Content-Type",
             "value":"text/plain; charset=\"UTF-8\""
           },
           { 
             "name":"Content-Transfer-Encoding",
             "value":"base64"
           },
           { 
             "name":"Date",
             "value":"Wed, 27 Jan 2016 14:05:45 +0000"
           }
         ],
         "commonHeaders":{ 
           "from":[ 
              "John Doe <john@example.com>"
           ],
           "date":"Wed, 27 Jan 2016 14:05:45 +0000",
           "to":[ 
              "Jane Doe <jane@example.com>, Mary Doe <mary@example.com>, Richard Doe <richard@example.com>"
           ],
           "messageId":"custom-message-ID",
           "subject":"Hello"
         }
      }
   }
```

### Complaint Notification Without a Feedback Report<a name="notification-examples-complaint-no-feedback"></a>

The following is an example of a complaint notification that includes the original email headers but does not include a feedback report\. When complaint notifications are not configured to include the original email headers, the `mail` object within the notifications does not include the `headersTruncated`, `headers`, and `commonHeaders` fields\.

```
   {
      "notificationType":"Complaint",
      "complaint":{
         "complainedRecipients":[
            {
               "emailAddress":"richard@example.com"
            }
         ],
         "timestamp":"2016-01-27T14:59:38.237Z",
         "feedbackId":"0000013786031775-fea503bc-7497-49e1-881b-a0379bb037d3-000000"
      },
      "mail":{
         "timestamp":"2016-01-27T14:59:38.237Z",
         "messageId":"0000013786031775-163e3910-53eb-4c8e-a04a-f29debf88a84-000000",
         "source":"john@example.com",
         "sourceArn": "arn:aws:ses:us-west-2:888888888888:identity/example.com",
         "sourceIp": "127.0.3.0",
         "sendingAccountId":"123456789012",
         "destination":[
            "jane@example.com",
            "mary@example.com",
            "richard@example.com"
         ],
         "headersTruncated":false,
         "headers":[ 
          { 
            "name":"From",
            "value":"\"John Doe\" <john@example.com>"
          },
          { 
            "name":"To",
            "value":"\"Jane Doe\" <jane@example.com>, \"Mary Doe\" <mary@example.com>, \"Richard Doe\" <richard@example.com>"
          },
          { 
            "name":"Message-ID",
            "value":"custom-message-ID"
          },
          { 
            "name":"Subject",
            "value":"Hello"
          },
          { 
            "name":"Content-Type",
            "value":"text/plain; charset=\"UTF-8\""
          },
          { 
            "name":"Content-Transfer-Encoding",
            "value":"base64"
          },
          { 
            "name":"Date",
            "value":"Wed, 27 Jan 2016 14:05:45 +0000"
          }
          ],
          "commonHeaders":{ 
             "from":[ 
                "John Doe <john@example.com>"
             ],
             "date":"Wed, 27 Jan 2016 14:05:45 +0000",
             "to":[ 
                "Jane Doe <jane@example.com>, Mary Doe <mary@example.com>, Richard Doe <richard@example.com>"
             ],
             "messageId":"custom-message-ID",
             "subject":"Hello"
          }
       }
   }
```

## Amazon SNS Delivery Notification Example<a name="notification-examples-delivery"></a>

The following is an example of a delivery notification that includes the original email headers\. When delivery notifications are not configured to include the original email headers, the `mail` object within the notifications does not include the `headersTruncated`, `headers`, and `commonHeaders` fields\.

```
   {
      "notificationType":"Delivery",
      "mail":{
         "timestamp":"2016-01-27T14:59:38.237Z",
         "messageId":"0000014644fe5ef6-9a483358-9170-4cb4-a269-f5dcdf415321-000000",
         "source":"john@example.com",
         "sourceArn": "arn:aws:ses:us-west-2:888888888888:identity/example.com",
         "sourceIp": "127.0.3.0",
         "sendingAccountId":"123456789012",
         "destination":[
            "jane@example.com"
         ], 
          "headersTruncated":false,
          "headers":[ 
           { 
              "name":"From",
              "value":"\"John Doe\" <john@example.com>"
           },
           { 
              "name":"To",
              "value":"\"Jane Doe\" <jane@example.com>"
           },
           { 
              "name":"Message-ID",
              "value":"custom-message-ID"
           },
           { 
              "name":"Subject",
              "value":"Hello"
           },
           { 
              "name":"Content-Type",
              "value":"text/plain; charset=\"UTF-8\""
           },
           { 
              "name":"Content-Transfer-Encoding",
              "value":"base64"
           },
           { 
              "name":"Date",
              "value":"Wed, 27 Jan 2016 14:58:45 +0000"
           }
          ],
          "commonHeaders":{ 
            "from":[ 
               "John Doe <john@example.com>"
            ],
            "date":"Wed, 27 Jan 2016 14:58:45 +0000",
            "to":[ 
               "Jane Doe <jane@example.com>"
            ],
            "messageId":"custom-message-ID",
            "subject":"Hello"
          }
       },
      "delivery":{
         "timestamp":"2016-01-27T14:59:38.237Z",
         "recipients":["jane@example.com"],
         "processingTimeMillis":546,     
         "reportingMTA":"a8-70.smtp-out.amazonses.com",
         "smtpResponse":"250 ok:  Message 64111812 accepted",
         "remoteMtaIp":"127.0.2.0"
      } 
   }
```