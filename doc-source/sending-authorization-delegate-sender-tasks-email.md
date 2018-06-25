# Sending Emails for the Identity Owner for Amazon SES Sending Authorization<a name="sending-authorization-delegate-sender-tasks-email"></a>

As a delegate sender, you send emails the same way that other Amazon SES senders do, except that you provide the ARN of the identity that the identity owner has authorized you to use\. When you call Amazon SES to send the email, Amazon SES checks to see if the identity that you specified has a policy that authorizes you to send for it\.

There are different ways that you can specify the identity's ARN when you send an email\. The method that you can use depends on whether you send the email by using the Amazon SES API operations or the Amazon SES SMTP interface\.

**Important**  
To successfully send an email from an identity owner's identity, you must connect to the Amazon SES endpoint of the AWS Region in which the identity is verified\. The sending authorization policy that grants you permission must be attached to the identity in that region\.

## Using the Amazon SES API<a name="sending-authorization-delegate-sender-tasks-api"></a>

As with any Amazon SES email sender, if you access Amazon SES through the Amazon SES API \(either directly through HTTPS or indirectly through an AWS SDK\), you can choose between one of two email\-sending actions: `SendEmail` and `SendRawEmail`\. The [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/) describes the details of these APIs, but we provide an overview of the sending authorization parameters here\.

### SendRawEmail<a name="sending-authorization-delegate-sender-tasks-api-sendrawemail"></a>

If you want to use `SendRawEmail` so that you can control the format of your emails, you can specify the cross\-account identity in one of two ways:
+ **Pass optional parameters to the `SendRawEmail` API**\. The required parameters are described in the following table:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization-delegate-sender-tasks-email.html)
+ **Include X\-headers in the email**\. X\-headers are custom headers that you can use in addition to standard email headers \(such as the From, Reply\-To, or Subject headers\)\. Amazon SES recognizes three X\-headers that you can use to specify sending authorization parameters:
**Important**  
Do not include these X\-headers in the DKIM signature, because they are removed by Amazon SES before sending the email\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization-delegate-sender-tasks-email.html)

  Amazon SES removes all X\-headers from the email before sending it\. If you include multiple instances of an X\-header, Amazon SES only uses the first instance\.

  The following example shows an email that includes sending authorization X\-headers:

  ```
   1. X-SES-SOURCE-ARN: arn:aws:ses:us-west-2:123456789012:identity/example.com
   2. X-SES-FROM-ARN: arn:aws:ses:us-west-2:123456789012:identity/example.com
   3. X-SES-RETURN-PATH-ARN: arn:aws:ses:us-west-2:123456789012:identity/example.com
   4. 
   5. From: sender@example.com
   6. To: recipient@example.com
   7. Return-Path: feedback@example.com
   8. Subject: subject
   9. Content-Type: multipart/alternative;
  10. 	boundary="----=_boundary"
  11. 
  12. ------=_boundary
  13. Content-Type: text/plain; charset=UTF-8
  14. Content-Transfer-Encoding: 7bit
  15. 
  16. body
  17. ------=_boundary
  18. Content-Type: text/html; charset=UTF-8
  19. Content-Transfer-Encoding: 7bit
  20. 
  21. body
  22. ------=_boundary--
  ```

### SendEmail<a name="sending-authorization-delegate-sender-tasks-api-sendemail"></a>

If you use the `SendEmail` operation, you can specify the cross\-account identity by passing in the optional parameters below\. You cannot use the X\-header method when you use the `SendEmail`operation\.


****  

| Parameter | Description | 
| --- | --- | 
| `SourceArn` | The ARN of the identity that is associated with the sending authorization policy that permits you to send for the email address specified in the `Source` parameter of `SendRawEmail`\. | 
| `ReturnPathArn` | The ARN of the identity that is associated with the sending authorization policy that permits you to use the email address specified in the `ReturnPath` parameter of `SendRawEmail`\. | 

The following example shows how to send an email that includes the `SourceArn` and `ReturnPathArn` attributes using the `SendEmail` operation and the [SDK for Python](https://aws.amazon.com/sdk-for-python)\.

```
import boto3
from botocore.exceptions import ClientError

# Create a new SES resource and specify a region.
client = boto3.client('ses',region_name="us-west-2")

# Try to send the email.
try:
    #Provide the contents of the email.
    response = client.send_email(
        Destination={
            'ToAddresses': [
                'recipient@example.com',
            ],
        },
        Message={
            'Body': {
                'Html': {
                    'Charset': 'UTF-8',
                    'Data': 'This email was sent with Amazon SES.',
                },
            },
            'Subject': {
                'Charset': 'UTF-8',
                'Data': 'Amazon SES Test',
            },
        },
        SourceArn='arn:aws:ses:us-west-2:123456789012:identity/example.com',
        ReturnPathArn='arn:aws:ses:us-west-2:123456789012:identity/example.com',
        Source='sender@example.com',
        ReturnPath='feedback@example.com'
    )
# Display an error if something goes wrong.	
except ClientError as e:
    print(e.response['Error']['Message'])
else:
    print("Email sent! Message ID:"),
    print(response['ResponseMetadata']['RequestId'])
```

## Using the Amazon SES SMTP interface<a name="sending-authorization-delegate-sender-tasks-smtp"></a>

When you use the Amazon SES SMTP interface for cross\-account sending, you have to include the `X-SES-SOURCE-ARN`, `X-SES-FROM-ARN` and `X-SES-RETURN-PATH-ARN` headers in your message\. Pass these headers after you issue the `DATA` command in the SMTP conversation\.