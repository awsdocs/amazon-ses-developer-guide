# Examples of Notifications for Amazon SES Email Receiving<a name="receiving-email-notifications-examples"></a>

This section includes examples of the following types of notifications:
+ [A notification sent as a result of an SNS action\.](#receiving-email-notifications-examples-sns-action)
+ [A notification sent as a result of another type of action](#receiving-email-notifications-examples-alert) \(an *alert notification*\)\.

## Notification of an SNS action<a name="receiving-email-notifications-examples-sns-action"></a>

This section contains an example of an SNS action notification\. Unlike the alert notification shown previously, it includes a `content` section that contains the email, which is typically in Multipurpose Internet Mail Extensions \(MIME\) format\.

```json
{
   "notificationType" : "Received",
   "receipt" : {
      "virusVerdict" : {
         "status" : "PASS"
      },
      "processingTimeMillis" : 222,
      "spamVerdict" : {
         "status" : "PASS"
      },
      "action" : {
         "topicArn" : "arn:aws:sns:us-east-1:012345678912:example-topic",
         "type" : "SNS"
      },
      "dkimVerdict" : {
         "status" : "PASS"
      },
      "timestamp" : "2015-09-11T20:32:33.936Z",
      "spfVerdict" : {
         "status" : "PASS"
      },
      "recipients" : [
         "recipient@example.com"
      ]
   },
   "mail" : {
      "headersTruncated" : false,
      "headers" : [
         {
            "value" : "<0000014fbe1c09cf-7cb9f704-7531-4e53-89a1-5fa9744f5eb6-000000@amazonses.com>",
            "name" : "Return-Path"
         },
         {
            "name" : "Received",
            "value" : "from a9-183.smtp-out.amazonses.com (a9-183.smtp-out.amazonses.com [54.240.9.183]) by inbound-smtp.us-east-1.amazonaws.com with SMTP id d6iitobk75ur44p8kdnnp7g2n800 for recipient@example.com; Fri, 11 Sep 2015 20:32:33 +0000 (UTC)"
         },
         {
            "value" : "v=1; a=rsa-sha256; q=dns/txt; c=relaxed/simple; s=ug7nbtf4gccmlpwj322ax3p6ow6yfsug; d=amazonses.com; t=1442003552; h=From:To:Subject:MIME-Version:Content-Type:Content-Transfer-Encoding:Date:Message-ID:Feedback-ID; bh=DWr3IOmYWoXCA9ARqGC/UaODfghffiwFNRIb2Mckyt4=; b=p4ukUDSFqhqiub+zPR0DW1kp7oJZakrzupr6LBe6sUuvqpBkig56UzUwc29rFbJF hlX3Ov7DeYVNoN38stqwsF8ivcajXpQsXRC1cW9z8x875J041rClAjV7EGbLmudVpPX 4hHst1XPyX5wmgdHIhmUuh8oZKpVqGi6bHGzzf7g=",
            "name" : "DKIM-Signature"
         },
         {
            "value" : "sender@example.com",
            "name" : "From"
         },
         {
            "name" : "To",
            "value" : "recipient@example.com"
         },
         {
            "value" : "Example subject",
            "name" : "Subject"
         },
         {
            "name" : "MIME-Version",
            "value" : "1.0"
         },
         {
            "value" : "text/plain; charset=UTF-8",
            "name" : "Content-Type"
         },
         {
            "name" : "Content-Transfer-Encoding",
            "value" : "7bit"
         },
         {
            "value" : "Fri, 11 Sep 2015 20:32:32 +0000",
            "name" : "Date"
         },
         {
            "name" : "Message-ID",
            "value" : "<61967230-7A45-4A9D-BEC9-87CBCF2211C9@example.com>"
         },
         {
            "name" : "X-SES-Outgoing",
            "value" : "2015.09.11-54.240.9.183"
         },
         {
            "value" : "1.us-east-1.Krv2FKpFdWV+KUYw3Qd6wcpPJ4Sv/pOPpEPSHn2u2o4=:AmazonSES",
            "name" : "Feedback-ID"
         }
      ],
      "destination" : [
         "recipient@example.com"
      ],
      "messageId" : "d6iitobk75ur44p8kdnnp7g2n800",
      "source" : "61967230-7A45-4A9D-BEC9-87CBCF2211C9@example.com",
      "commonHeaders" : {
         "subject" : "Example subject",
         "to" : [
            "recipient@example.com"
         ],
         "returnPath" : "0000014fbe1c09cf-7cb9f704-7531-4e53-89a1-5fa9744f5eb6-000000@amazonses.com",
         "date" : "Fri, 11 Sep 2015 20:32:32 +0000",
         "messageId" : "<61967230-7A45-4A9D-BEC9-87CBCF2211C9@example.com>",
         "from" : [
            "sender@example.com"
         ]
      },
      "timestamp" : "2015-09-11T20:32:33.936Z"
   },
   "content" : "Return-Path: <61967230-7A45-4A9D-BEC9-87CBCF2211C9@example.com>\r\nReceived: from a9-183.smtp-out.amazonses.com (a9-183.smtp-out.amazonses.com [54.240.9.183])\r\n by inbound-smtp.us-east-1.amazonaws.com with SMTP id d6iitobk75ur44p8kdnnp7g2n800\r\n for recipient@example.com;\r\n Fri, 11 Sep 2015 20:32:33 +0000 (UTC)\r\nDKIM-Signature: v=1; a=rsa-sha256; q=dns/txt; c=relaxed/simple;\r\n\ts=ug7nbtf4gccmlpwj322ax3p6ow6yfsug; d=amazonses.com; t=1442003552;\r\n\th=From:To:Subject:MIME-Version:Content-Type:Content-Transfer-Encoding:Date:Message-ID:Feedback-ID;\r\n\tbh=DWr3IOmYWoXCA9ARqGC/UaODfghffiwFNRIb2Mckyt4=;\r\n\tb=p4ukUDSFqhqiub+zPR0DW1kp7oJZakrzupr6LBe6sUuvqpBkig56UzUwc29rFbJF\r\n\thlX3Ov7DeYVNoN38stqwsF8ivcajXpQsXRC1cW9z8x875J041rClAjV7EGbLmudVpPX\r\n\t4hHst1XPyX5wmgdHIhmUuh8oZKpVqGi6bHGzzf7g=\r\nFrom: sender@example.com\r\nTo: recipient@example.com\r\nSubject: Example subject\r\nMIME-Version: 1.0\r\nContent-Type: text/plain; charset=UTF-8\r\nContent-Transfer-Encoding: 7bit\r\nDate: Fri, 11 Sep 2015 20:32:32 +0000\r\nMessage-ID: <61967230-7A45-4A9D-BEC9-87CBCF2211C9@example.com>\r\nX-SES-Outgoing: 2015.09.11-54.240.9.183\r\nFeedback-ID: 1.us-east-1.Krv2FKpFdWV+KUYw3Qd6wcpPJ4Sv/pOPpEPSHn2u2o4=:AmazonSES\r\n\r\nExample content\r\n"
}
```

## Alert Notification<a name="receiving-email-notifications-examples-alert"></a>

This section contains an example of an Amazon SNS notification that can be triggered by an S3 action\. Notifications triggered by Lambda actions, bounce actions, stop actions, and WorkMail actions are similar\. Although the notification contains information about the email, it does not contain the content of the email itself\.

```json
{
   "notificationType" : "Received",
   "receipt" : {
      "processingTimeMillis" : 406,
      "virusVerdict" : {
         "status" : "PASS"
      },
      "action" : {
         "topicArn" : "arn:aws:sns:us-east-1:012345678912:example-topic",
         "bucketName" : "my-S3-bucket",
         "objectKey" : "email/",
         "type" : "S3"
      },
      "dkimVerdict" : {
         "status" : "PASS"
      },
      "timestamp" : "2015-09-11T20:32:33.936Z",
      "spfVerdict" : {
         "status" : "PASS"
      },
      "spamVerdict" : {
         "status" : "PASS"
      },
      "recipients" : [
         "recipient@example.com"
      ]
   },
   "mail" : {
      "destination" : [
         "recipient@example.com"
      ],
      "headers" : [
         {
            "name" : "Return-Path",
            "value" : "<0000014fbe1c09cf-7cb9f704-7531-4e53-89a1-5fa9744f5eb6-000000@amazonses.com>"
         },
         {
            "name" : "Received",
            "value" : "from a9-183.smtp-out.amazonses.com (a9-183.smtp-out.amazonses.com [54.240.9.183]) by inbound-smtp.us-east-1.amazonaws.com with SMTP id d6iitobk75ur44p8kdnnp7g2n800 for recipient@example.com; Fri, 11 Sep 2015 20:32:33 +0000 (UTC)"
         },
         {
            "value" : "v=1; a=rsa-sha256; q=dns/txt; c=relaxed/simple; s=ug7nbtf4gccmlpwj322ax3p6ow6yfsug; d=amazonses.com; t=1442003552; h=From:To:Subject:MIME-Version:Content-Type:Content-Transfer-Encoding:Date:Message-ID:Feedback-ID; bh=DWr3IOmYWoXCA9ARqGC/UaODfghffiwFNRIb2Mckyt4=; b=p4ukUDSFqhqiub+zPR0DW1kp7oJZakrzupr6LBe6sUuvqpBkig56UzUwc29rFbJF hlX3Ov7DeYVNoN38stqwsF8ivcajXpQsXRC1cW9z8x875J041rClAjV7EGbLmudVpPX 4hHst1XPyX5wmgdHIhmUuh8oZKpVqGi6bHGzzf7g=",
            "name" : "DKIM-Signature"
         },
         {
            "value" : "sender@example.com",
            "name" : "From"
         },
         {
            "name" : "To",
            "value" : "recipient@example.com"
         },
         {
            "value" : "Example subject",
            "name" : "Subject"
         },
         {
            "value" : "1.0",
            "name" : "MIME-Version"
         },
         {
            "value" : "text/plain; charset=UTF-8",
            "name" : "Content-Type"
         },
         {
            "value" : "7bit",
            "name" : "Content-Transfer-Encoding"
         },
         {
            "name" : "Date",
            "value" : "Fri, 11 Sep 2015 20:32:32 +0000"
         },
         {
            "name" : "Message-ID",
            "value" : "<61967230-7A45-4A9D-BEC9-87CBCF2211C9@example.com>"
         },
         {
            "value" : "2015.09.11-54.240.9.183",
            "name" : "X-SES-Outgoing"
         },
         {
            "value" : "1.us-east-1.Krv2FKpFdWV+KUYw3Qd6wcpPJ4Sv/pOPpEPSHn2u2o4=:AmazonSES",
            "name" : "Feedback-ID"
         }
      ],
      "messageId" : "d6iitobk75ur44p8kdnnp7g2n800",
      "source" : "0000014fbe1c09cf-7cb9f704-7531-4e53-89a1-5fa9744f5eb6-000000@amazonses.com",
      "commonHeaders" : {
         "messageId" : "<61967230-7A45-4A9D-BEC9-87CBCF2211C9@example.com>",
         "subject" : "Example subject",
         "date" : "Fri, 11 Sep 2015 20:32:32 +0000",
         "from" : [
            "sender@example.com"
         ],
         "returnPath" : "0000014fbe1c09cf-7cb9f704-7531-4e53-89a1-5fa9744f5eb6-000000@amazonses.com",
         "to" : [
            "recipient@example.com"
         ]
      },
      "timestamp" : "2015-09-11T20:32:33.936Z",
      "headersTruncated" : false
   }
}
```
