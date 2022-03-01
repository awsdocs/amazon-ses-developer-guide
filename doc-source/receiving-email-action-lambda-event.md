# Sample incoming email event<a name="receiving-email-action-lambda-event"></a>

There are two ways to send incoming email events to a Lambda function\. The first method is to use a Lambda action in your receipt rule to send the event record directly to the function\. The second method is to use an Amazon SNS action in your receipt rule to send the event records to Amazon SNS, and then add the Lambda function as a subscribing endpoint to the Amazon SNS topic\.

This section contains examples of the event records that Amazon SES can send to Lambda\. You can use these examples to create and test Lambda functions\.

**Note**  
The examples in this section include line breaks to make them easier to read\. If you copy the examples in this section, you should remove the additional line breaks to produce valid JSON objects\.

## Event records provided by the Lambda action<a name="receiving-email-action-lambda-event-lambdaaction"></a>

When you add a Lambda action to a receipt rule, Amazon SES sends an event record to Lambda every time it receives an incoming message\. This event contains information about several of the email headers for the incoming message, as well as the results of several tests that Amazon SES performs on incoming messages\. However, it omits the body of the incoming email\.

The following example shows the values that these event records typically contain\.

```
{
  "Records": [{
    "eventSource": "aws:ses",
    "eventVersion": "1.0",
    "ses": {
      "mail": {
        "timestamp": "2019-08-05T21:30:02.028Z",
        "source": "prvs=144d0cba7=sender@example.com",
        "messageId": "EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000",
        "destination": ["recipient@example.com"],
        "headersTruncated": false,
        "headers": [{
          "name": "Return-Path",
          "value": "<prvs=144d0cba7=sender@example.com>"
        }, {
          "name": "Received",
          "value": "from smtp.example.com [203.0.113.0]) by inbound-smtp.us-east-1.amazonaws.com 
                    with SMTP id bsvpsoklfhu7u50iur7h0kk9a2ou0r7iexample for recipient@example.com;
                    Mon, 05 Aug 2019 21:30:02 +0000 (UTC)"
        }, {
          "name": "X-SES-Spam-Verdict",
          "value": "PASS"
        }, {
          "name": "X-SES-Virus-Verdict",
          "value": "PASS"
        }, {
          "name": "Received-SPF",
          "value": "pass (spfCheck: domain of example.com designates 203.0.113.0 as permitted sender) 
                    client-ip=203.0.113.0; envelope-from=prvs=144d0cba42=sender@example.com; helo=
                    smtp.example.com;"
        }, {
          "name": "Authentication-Results",
          "value": "amazonses.com; spf=pass (spfCheck: domain of example.com designates 203.0.113.0
                    as permitted sender) client-ip=203.0.113.0; envelope-from=prvs=144d0cba42=
                    sender@example.com; helo=smtp.example.com; dkim=pass header.i=@example.com; 
                    dmarc=none header.from=example.com;"
        }, {
          "name": "X-SES-RECEIPT",
          "value": "AEFBQUFBQUFBQUFHbFo0VU81VzVuYmRDNm51nhTVWpabDh6J4V2l5cG5PSHFtNzlBeUk90example"
        }, {
          "name": "X-SES-DKIM-SIGNATURE",
          "value": "a=rsa-sha256; q=dns/txt; b=Cm1emU30VcD6example=; c=relaxed/simple; s=6gbrjpgwjs
                    5zn6fwqknexample; d=amazonses.com; t=1567719002; v=1; bh=DSofsjAoUvyZj6YsBDP5en
                    pRO1otGb7Nes0Qexample=; h=From:To:Cc:Bcc:Subject:Date:Message-ID:MIME-Version:
                    Content-Type:X-SES-RECEIPT;"
        }, {
          "name": "DKIM-Signature",
          "value": "v=1; a=rsa-sha256; c=relaxed/relaxed; d=example.com; i=@example.com; q=dns/txt; 
                    s=example12345; t=1567719001; x=1599255001; h=from:to:subject:date:message-id:
                    references:in-reply-to:mime-version; bh=sjAoUvyZj6YsBDP5enpRO1otGb7s0Qexample=; 
                    b=EQw2D4RLOW2IHE9OgfEA4WXp+AENJtaD2+63wmd5J+d+t/xoaiKUGClOS7WhpyOmlipryOz+iOhxU
                    v350xJIHjLTi9Jsnlw76mRK8o4770TaUz620joCVN21n4cxsrRZpv+1kS0EcAxaF30pmwlni+XT4ems
                    Vxn7zO0I8example=;"
        }, {
          "name": "Received",
          "value": "from mail.example.com (mail.example.com [203.0.113.0]) by email-inbound-relay-
                    1d-9ec21598.us-east-1.example.com (Postfix) with ESMTPS id 57F83A2042 for 
                    <recipient@example.com>; Mon, 5 Aug 2019 21:29:58 +0000 (UTC)"
        }, {
          "name": "From",
          "value": "\"Doe, John\" <sender@example.com>"
        }, {
          "name": "To",
          "value": "\"recipient@example.com\" <recipient@example.com>"
        }, {
          "name": "Subject",
          "value": "This is a test"
        }, {
          "name": "Thread-Topic",
          "value": "This is a test"
        }, {
          "name": "Thread-Index",
          "value": "AQHVZDAaQ58yKI8q7kaAjkhC5stGexample"
        }, {
          "name": "Date",
          "value": "Mon, 5 Aug 2019 21:29:57 +0000"
        }, {
          "name": "Message-ID",
          "value": "<F8098FDD-49A3-442D-9935-F6112example@example.com>"
        }, {
          "name": "References",
          "value": "<1FCED16B-F6B0-4506-A6F0-594DFexample@example.com>"
        }, {
          "name": "In-Reply-To",
          "value": "<1FCED16B-F6B0-4506-A6F0-594DFexample@example.com>"
        }, {
          "name": "Accept-Language",
          "value": "en-US"
        }, {
          "name": "Content-Language",
          "value": "en-US"
        }, {
          "name": "X-MS-Has-Attach",
          "value": ""
        }, {
          "name": "X-MS-TNEF-Correlator",
          "value": ""
        }, {
          "name": "x-ms-exchange-messagesentrepresentingtype",
          "value": "1"
        }, {
          "name": "x-ms-exchange-transport-fromentityheader",
          "value": "Hosted"
        }, {
          "name": "x-originating-ip",
          "value": "[203.0.113.0]"
        }, {
          "name": "Content-Type",
          "value": "multipart/alternative; boundary=\"_000_F8098FDD49A344F6112B195BDAexamplecom_\""
        }, {
          "name": "MIME-Version",
          "value": "1.0"
        }, {
          "name": "Precedence",
          "value": "Bulk"
        }],
        "commonHeaders": {
          "returnPath": "prvs=144d0cba7=sender@example.com",
          "from": ["\"Doe, John\" <sender@example.com>"],
          "date": "Mon, 5 Aug 2019 21:29:57 +0000",
          "to": ["\"recipient@example.com\" <recipient@example.com>"],
          "messageId": "<F8098FDD-49A3-442D-9935-F6112B195BDA@example.com>",
          "subject": "This is a test"
        }
      },
      "receipt": {
        "timestamp": "2019-08-05T21:30:02.028Z",
        "processingTimeMillis": 1205,
        "recipients": ["recipient@example.com"],
        "spamVerdict": {
          "status": "PASS"
        },
        "virusVerdict": {
          "status": "PASS"
        },
        "spfVerdict": {
          "status": "PASS"
        },
        "dkimVerdict": {
          "status": "PASS"
        },
        "dmarcVerdict": {
          "status": "GRAY"
        },
        "action": {
          "type": "Lambda",
          "functionArn": "arn:aws:lambda:us-east-1:123456789012:function:IncomingEmail",
          "invocationType": "Event"
        }
      }
    }
  }]
}
```

## Event records provided by the Amazon SNS action<a name="receiving-email-action-lambda-event-snsaction"></a>

When you add an Amazon SNS action to your receipt rule, the notification contains the entire contents of the email\. If you want to have a Lambda function process the body of the email, you should instead add an Amazon SNS action to the receipt rule\. Then, in Amazon SNS, subscribe your Lambda function to the Amazon SNS function\. This configuration causes your Lambda function to be activated when it receives a notification from the Amazon SNS topic\.



```
{
    'Records': [
        {
            'EventSource': 'aws:sns',
            'EventVersion': '1.0',
            'EventSubscriptionArn': 'arn:aws:sns:us-east-1:123456789012:IncomingEmail:12345678',
            'Sns': {
                'Type': 'Notification',
                'MessageId': 'EXAMPLE7c191be45-e9aedb9a-02f9-4d12-a87d-dd0099a07f8a-000000',
                'TopicArn': 'arn:aws:sns:us-east-1:123456789012:IncomingEmail',
                'Subject': 'Amazon SES Email Receipt Notification',
                'Message': <message content—see below>,
                'Timestamp': '2019-09-06T18:52:16.076Z',
                'SignatureVersion': '1',
                'Signature': '012345678901example==',
                'SigningCertUrl': 'https://sns.us-east-1.amazonaws.com/SimpleNotificationService
                                   -01234567890123456789012345678901.pem',
                'UnsubscribeUrl': 'https://sns.us-east-1.amazonaws.com/?Action=Unsubscribe&
                                   SubscriptionArn=arn:aws:sns:us-east-1:0123456789012:IncomingEmail:
                                   0b863538-3f32-462e-9c89-8d8e0example',
                'MessageAttributes': {}
            }
        }
    ]
}
```

The `Message` attribute contains a JSON\-encoded string\. This string contains the headers and content of the message\. The message body itself is base64 encoded\. If you want to use the message body in your Lambda function, you first have to decode the `Message` attribute, and then decode the `Content` object\.

The following example shows the values that are contained in the `Message` attribute\.

```
{
  "notificationType": "Received",
  "mail": {
    "timestamp": "2019-09-06T18:52:14.965Z",
    "source": "0100016d07eb7477-8e1938ce-475e-4e4b-89cb-example-000000@amazonses.com",
    "messageId": "12345678901example",
    "destination": ["recipient@example.com"],
    "headersTruncated": false,
    "headers": [{
      "name": "Return-Path",
      "value": "<0100016d07eb7477-8e1938ce-475e-4e4b-89cb-example-000000@amazonses.com>"
    }, {
      "name": "Received",
      "value": "from a1-23.smtp-out.amazonses.com (a1-23.smtp-out.amazonses.com [203.0.113.0]) by
                inbound-smtp.us-east-1.amazonaws.com with SMTP id
                12345678901example for recipient@example.com; Fri, 06 Sep 2019
                18:52:14 +0000 (UTC)"
    }, {
      "name": "X-SES-Spam-Verdict",
      "value": "PASS"
    }, {
      "name": "X-SES-Virus-Verdict",
      "value": "PASS"
    }, {
      "name": "Received-SPF",
      "value": "pass (spfCheck: domain of amazonses.com designates 203.0.113.0 as permitted sender)
                client-ip=203.0.113.0; envelope-from=0100016d07eb7477-8e1938ce-475e-4e4b-89cb-example
                -000000@amazonses.com; helo=a1-23.smtp-out.amazonses.com;"
    }, {
      "name": "Authentication-Results",
      "value": "amazonses.com; spf=pass (spfCheck: domain of amazonses.com designates 203.0.113.0
                as permitted sender) client-ip=203.0.113.0; envelope-from=0100016d07eb7477-8e1938ce
                -475e-4e4b-89cb-example-000000@amazonses.com; helo=a1-23.smtp-out.amazonses.com;
                dkim=pass header.i=@amazonses.com; dmarc=none header.from=example.com;"
    }, {
      "name": "X-SES-RECEIPT",
      "value": "AEFBQUFBQUFBQUFFQkx0QUJZZENEXAMPLE="
    }, {
      "name": "X-SES-DKIM-SIGNATURE",
      "value": "a=rsa-sha256; q=dns/txt; b=d5azwgA2iBqAjA4NBm1ARzjJ95raRmy4G84iVdd3x2JzSHeUnQuTuLmJ
                AqRrYY3WpMIVRFy01hITaguCVjUPWBR0xF6fCEXH85cf3RNeFQyLfWZqoXKfBdjFRV+13troDterH2MxBUL
                8rjzcvdHetl0ImwlaK2PGmePTexample=; c=relaxed/simple; s=EXAMPLE7c191be45-e9aedb9a-02
                f9-4d12-a87d-dd0099a07f8a-000000; d=amazonses.com; t=1567795935; v=1; bh=CZ1SghsYaA
                6SSCbitzsLISeFoNlpdtH1Pyiexample=; h=From:To:Cc:Bcc:Subject:Date:Message-ID:MIME-
                Version:Content-Type:X-SES-RECEIPT;"
    }, {
      "name": "DKIM-Signature",
      "value": "v=1; a=rsa-sha256; q=dns/txt; c=relaxed/simple; s=EXAMPLE7c191be45-e9aedb9a-02f9-
                4d12-a87d-dd0099a07f8a-000000; d=amazonses.com; t=1567795934; h=From:To:Subject:
                MIME-Version:Content-Type:Message-ID:Date:Feedback-ID; bh=CZ1SghsYaA6SSCbitzsLISeFo
                NlpdtH1Pyiexample=; b=L6VXqR1PSN/FYqJI/VAfPRKFgtakcHCYJvuJqVYbuJT8I3FOhqOvkbcgHxOgs
                woxPfvGrL6S53H8Er5Do/CPvOM4Tx3ilE+a0GTYVLjKmwltNeN09YWlJAoqG5KMQPZUxRYaNvYPInLzUdGi
                rdjkbSIgZEnrvq5MzaMWexample="
    }, {
      "name": "From",
      "value": "sender@example.com"
    }, {
      "name": "To",
      "value": "recipient@example.com"
    }, {
      "name": "Subject",
      "value": "Amazon SES Test"
    }, {
      "name": "MIME-Version",
      "value": "1.0"
    }, {
      "name": "Content-Type",
      "value": "multipart/alternative;  boundary=\"----=_Part_869787_396523212.15677example\""
    }, {
      "name": "Message-ID",
      "value": "<0100016d07eb7477-8e1938ce-475e-4e4b-89cb-example-000000@email.amazonses.com>"
    }, {
      "name": "Date",
      "value": "Fri, 6 Sep 2019 18:52:14 +0000"
    }, {
      "name": "X-SES-Outgoing",
      "value": "2019.09.06-203.0.113.0"
    }, {
      "name": "Feedback-ID",
      "value": "1.us-east-1.ZitRoTk0xziun8WEJevt+cSJ17QNuCwulg2D2v3nrT0=:AmazonSES"
    }],
    "commonHeaders": {
      "returnPath": "0100016d07eb7477-8e1938ce-475e-4e4b-89cb-example-000000@amazonses.com",
      "from": ["sender@example.com"],
      "date": "Fri, 6 Sep 2019 18:52:14 +0000",
      "to": ["recipient@example.com"],
      "messageId": "<0100016d07eb7477-8e1938ce-475e-4e4b-89cb-example-000000@email.amazonses.com>",
      "subject": "Amazon SES Test"
    }
  },
  "receipt": {
    "timestamp": "2019-09-06T18:52:14.965Z",
    "processingTimeMillis": 1098,
    "recipients": ["recipient@example.com"],
    "spamVerdict": {
      "status": "PASS"
    },
    "virusVerdict": {
      "status": "PASS"
    },
    "spfVerdict": {
      "status": "PASS"
    },
    "dkimVerdict": {
      "status": "GRAY"
    },
    "dmarcVerdict": {
      "status": "GRAY"
    },
    "action": {
      "type": "SNS",
      "topicArn": "arn:aws:sns:us-east-1:123456789012:IncomingEmail",
      "encoding": "BASE64"
    }
  },
  "content": "UmV0dXJuLVBhdGg6IDwwMTAwMDE2ZDA3ZWI3NDc3LThlMTkzOGNlLTQ3NWUtNGU0Yi04OWNiLWV4YW1wbGUtM
              DAwMDAwQGFtYXpvbnNlcy5jb20+ClJlY2VpdmVkOiBmcm9tIGExLTIzLnNtdHAtb3V0LmFtYXpvbnNlcy5jb2
              0gKGExLTIzLnNtdHAtb3V0LmFtYXpvbnNlcy5jb20gWzIwMy4wLjExMy4wXSkKIGJ5IGluYm91bmQtc210cC5
              1cy1lYXN0LTEuYW1hem9uYXdzLmNvbSB3aXRoIFNNVFAgaWQgZW5xMTBpYW1lMXFjdTMxamg1ZGEyZ244OWlt
              dm90Mms2ZXhhbXBsZQogZm9yIHJlY2lwaWVudEBleGFtcGxlLmNvbTsKIEZyaSwgMDYgU2VwIDIwMTkgMTg6N
              TI6MTQgKzAwMDAgKFVUQykKWC1TRVMtU3BhbS1WZXJkaWN0OiBQQVNTClgtU0VTLVZpcnVzLVZlcmRpY3Q6IF
              BBU1MKUmVjZWl2ZWQtU1BGOiBwYXNzIChzcGZDaGVjazogZG9tYWluIG9mIGFtYXpvbnNlcy5jb20gZGVzaWd
              uYXRlcyAyMDMuMC4xMTMuMCBhcyBwZXJtaXR0ZWQgc2VuZGVyKSBjbGllbnQtaXA9MjAzLjAuMTEzLjA7IGVu
              dmVsb3BlLWZyb209MDEwMDAxNmQwN2ViNzQ3Ny04ZTE5MzhjZS00NzVlLTRlNGItODljYi1leGFtcGxlLTAwM
              DAwMEBhbWF6b25zZXMuY29tOyBoZWxvPWExLTIzLnNtdHAtb3V0LmFtYXpvbnNlcy5jb207CkF1dGhlbnRpY2
              F0aW9uLVJlc3VsdHM6IGFtYXpvbnNlcy5jb207CiBzcGY9cGFzcyAoc3BmQ2hlY2s6IGRvbWFpbiBvZiBhbWF
              6b25zZXMuY29tIGRlc2lnbmF0ZXMgMjAzLjAuMTEzLjAgYXMgcGVybWl0dGVkIHNlbmRlcikgY2xpZW50LWlw
              PTIwMy4wLjExMy4wOyBlbnZlbG9wZS1mcm9tPTAxMDAwMTZkMDdlYjc0NzctOGUxOTM4Y2UtNDc1ZS00ZTRiL
              Tg5Y2ItZXhhbXBsZS0wMDAwMDBAYW1hem9uc2VzLmNvbTsgaGVsbz1hMS0yMy5zbXRwLW91dC5hbWF6b25zZX
              MuY29tOwogZGtpbT1wYXNzIGhlYWRlci5pPUBhbWF6b25zZXMuY29tOwogZG1hcmM9bm9uZSBoZWFkZXIuZnJ
              vbT1leGFtcGxlLmNvbTsKWC1TRVMtUkVDRUlQVDogQUVGQlFVRkJRVUZCUVVGRlFreDBRVUpaWkVORVhBTVBM
              RT0KWC1TRVMtREtJTS1TSUdOQVRVUkU6IGE9cnNhLXNoYTI1NjsgcT1kbnMvdHh0OyBiPWQ1YXp3Z0EyaUJxQ
              WpBNE5CbTFBUnpqSjk1cmFSbXk0Rzg0aVZkZDN4Mkp6U0hlVW5RdVR1TG1KQXFScllZM1dwTUlWUkZ5MDFoSV
              RhZ3VDVmpVUFdCUjB4RjZmQ0VYSDg1Y2YzUk5lRlF5TGZXWnFvWEtmQmRqRlJWKzEzdHJvRHRlckgyTXhCVUw
              4cmp6Y3ZkSGV0bDBJbXdsYUsyUEdtZVBUZXhhbXBsZT07IGM9cmVsYXhlZC9zaW1wbGU7IHM9RVhBTVBMRTdj
              MTkxYmU0NS1lOWFlZGI5YS0wMmY5LTRkMTItYTg3ZC1kZDAwOTlhMDdmOGEtMDAwMDAwOyBkPWFtYXpvbnNlc
              y5jb207IHQ9MTU2Nzc5NTkzNTsgdj0xOyBiaD1DWjFTZ2hzWWFBNlNTQ2JpdHpzTElTZUZvTmxwZHRIMVB5aW
              V4YW1wbGU9OyBoPUZyb206VG86Q2M6QmNjOlN1YmplY3Q6RGF0ZTpNZXNzYWdlLUlEOk1JTUUtVmVyc2lvbjp
              Db250ZW50LVR5cGU6WC1TRVMtUkVDRUlQVDsKREtJTS1TaWduYXR1cmU6IHY9MTsgYT1yc2Etc2hhMjU2OyBx
              PWRucy90eHQ7IGM9cmVsYXhlZC9zaW1wbGU7CglzPUVYQU1QTEU3YzE5MWJlNDUtZTlhZWRiOWEtMDJmOS00Z
              DEyLWE4N2QtZGQwMDk5YTA3ZjhhLTAwMDAwMDsgZD1hbWF6b25zZXMuY29tOyB0PTE1Njc3OTU5MzQ7CgloPU
              Zyb206VG86U3ViamVjdDpNSU1FLVZlcnNpb246Q29udGVudC1UeXBlOk1lc3NhZ2UtSUQ6RGF0ZTpGZWVkYmF
              jay1JRDsKCWJoPUNaMVNnaHNZYUE2U1NDYml0enNMSVNlRm9ObHBkdEgxUHlpTWV4YW1wbGU9OwoJYj1leGFt
              cGxlPQpGcm9tOiBzZW5kZXJAZXhhbXBsZS5jb20KVG86IHJlY2lwaWVudEBleGFtcGxlLmNvbQpTdWJqZWN0O
              iBBbWF6b24gU0VTIFRlc3QKTUlNRS1WZXJzaW9uOiAxLjAKQ29udGVudC1UeXBlOiBtdWx0aXBhcnQvYWx0ZX
              JuYXRpdmU7IAoJYm91bmRhcnk9Ii0tLS09X1BhcnRfODY5Nzg3XzM5NjUyMzIxMi4xNTY3N2V4YW1wbGUiCk1
              lc3NhZ2UtSUQ6IDwwMTAwMDE2ZDA3ZWI3NDc3LThlMTkzOGNlLTQ3NWUtNGU0Yi04OWNiLWV4YW1wbGUtMDAw
              MDAwQGVtYWlsLmFtYXpvbnNlcy5jb20+CkRhdGU6IEZyaSwgNiBTZXAgMjAxOSAxODo1MjoxNCArMDAwMApYL
              VNFUy1PdXRnb2luZzogMjAxOS4wOS4wNi0yMDMuMC4xMTMuMApGZWVkYmFjay1JRDogMS51cy1lYXN0LTEuWm
              l0Um9UazB4eml1bjhXRUpldnQrZXhhbXBsZT06QW1hem9uU0VTCgotLS0tLS09X1BhcnRfODY5Nzg3XzM5NjU
              yMzIxMi4xNTY3N2V4YW1wbGUKQ29udGVudC1UeXBlOiB0ZXh0L3BsYWluOyBjaGFyc2V0PVVURi04CkNvbnRl
              bnQtVHJhbnNmZXItRW5jb2Rpbmc6IDdiaXQKCkFtYXpvbiBTRVMgVGVzdApUaGlzIGVtYWlsIHdhcyBzZW50I
              HdpdGggQW1hem9uIFNFUy4KLS0tLS0tPV9QYXJ0Xzg2OTc4N18zOTY1MjMyMTIuMTU2NzdleGFtcGxlCkNvbn
              RlbnQtVHlwZTogdGV4dC9odG1sOyBjaGFyc2V0PVVURi04CkNvbnRlbnQtVHJhbnNmZXItRW5jb2Rpbmc6IDd
              iaXQKCjxodG1sPgo8aGVhZD48L2hlYWQ+Cjxib2R5PgogIDxoMT5BbWF6b24gU0VTIFRlc3Q8L2gxPgogIDxw
              PlRoaXMgZW1haWwgd2FzIHNlbnQgd2l0aCBBbWF6b24gU0VTLjwvcD4KPGltZyBhbHQ9IiIgc3JjPSJodHRwO
              i8vZXhhbXBsZS5yLnVzLWVhc3QtMS5hd3N0cmFjay5tZS9JMC8wMTAwMDE2ZDA3ZWI3NDc3LThlMTkzOGNlLT
              Q3NWUtNGU0Yi04OWNiLWV4YW1wbGUtMDAwMDAwL3UtWUphaHRkTTJTclhZQ2QiIHN0eWxlPSJkaXNwbGF5OiB
              ub25lOyB3aWR0aDogMXB4OyBoZWlnaHQ6IDFweDsiPgo8L2JvZHk+CjwvaHRtbD4KICAgICAgICAgICAgCi0t
              LS0tLT1fUGFydF84Njk3ODdfMzk2NTIzMjEyLjE1Njc3ZXhhbXBsZS0tCg=="
}
```
