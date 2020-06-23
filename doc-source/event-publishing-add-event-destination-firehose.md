# Set up a Kinesis Data Firehose event destination for Amazon SES event publishing<a name="event-publishing-add-event-destination-firehose"></a>

An Amazon Kinesis Data Firehose event destination represents an entity that publishes specific Amazon SES email sending events to Kinesis Data Firehose\. Because a Kinesis Data Firehose event destination exists within a configuration set only, you first have to [create a configuration set](event-publishing-create-configuration-set.md)\. Next, you add the event destination to the configuration set\.

This section includes a procedure for creating an event destination by using the Amazon SES console\. You can also use the [UpdateConfigurationSetEventDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateConfigurationSetEventDestination.html) operation in the Amazon SES API V2 destination to create and update event destinations\. 

**To add a Kinesis Data Firehose event destination to a configuration set \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. Choose a configuration set from the configuration set list\. If the list is empty, you must first [create a configuration set](event-publishing-create-configuration-set.md)\.

1. For **Add Destination**, choose **Select a destination type**, and then choose **Kinesis Data Firehose**\.

1. For **Name**, type a name for the event destination\.

1. For **Event types**, select at least one event type to publish to the event destination:
   + **Sends** – The call to Amazon SES was successful and Amazon SES will attempt to deliver the email\.
   + **Rejects** – Amazon SES accepted the email, determined that it contained a virus, and rejected it\. Amazon SES didn't attempt to deliver the email to the recipient's mail server\.
   + **Bounces** – The recipient's mail server permanently rejected the email\. This event corresponds to hard bounces\. Soft bounces are only included when Amazon SES fails to deliver the email after retrying for a period of time\.
   + **Complaints** – The email was successfully delivered to the recipient\. The recipient marked the email as spam\.
   + **Deliveries** – Amazon SES successfully delivered the email to the recipient's mail server\.
   + **Opens** – The recipient received the message and opened it in their email client\.
   + **Clicks** – The recipient clicked one or more links in the email\.
   + **Rendering Failures** – The email wasn't sent because of a template rendering issue\. This event type only occurs when you send email using the [SendTemplatedEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html) or [SendBulkTemplatedEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html) API operations\. This event type can occur when template data is missing, or when there is a mismatch between template parameters and data\.
   + **Delivery Delays** – The email couldn't be delivered to the recipient because a temporary issue occurred\. Delivery delays can occur, for example, when the recipient's inbox is full, or when the receiving email server experiences a transient issue\.
**Note**  
To add the `DELIVERY_DELAY` event type to an event destination, you have to use the [ UpdateConfigurationSetEventDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateConfigurationSetEventDestination.html) operation in the Amazon SES API V2\. Currently, you can't add this event type to a configuration set by using the Amazon SES console\.

1. Select **Enabled**\.

1. For **Stream**, choose an existing Kinesis Data Firehose delivery stream, or choose **Create new stream** to create a new one using the Kinesis Data Firehose console\.

   For information about creating a stream using the Kinesis Data Firehose console, see [Creating an Amazon Kinesis Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.

1. For **IAM role**, choose an IAM role for which Amazon SES has permission to publish to Kinesis Data Firehose on your behalf\. You can choose an existing role, have Amazon SES create a role for you, or create your own role\.

   If you choose an existing role or create your own role, you must manually modify the role's policies to give the role permission to access the Kinesis Data Firehose delivery stream, and to give Amazon SES permission to assume the role\. For example policies, see [Giving Amazon SES Permission to Publish to Your Kinesis Data Firehose Delivery Stream](#event-publishing-add-event-destination-firehose-role)\. 

1. Choose **Save**\.

For information about how to use the `UpdateConfigurationSetEventDestination` API to add a Kinesis Data Firehose event destination, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_UpdateConfigurationSetEventDestination.html)\.

## Giving Amazon SES Permission to Publish to Your Kinesis Data Firehose Delivery Stream<a name="event-publishing-add-event-destination-firehose-role"></a>

To enable Amazon SES to publish records to your Kinesis Data Firehose delivery stream, you must use an AWS Identity and Access Management \(IAM\) [role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) and attach or modify the role's permissions policy and trust policy\. The permissions policy enables the role to publish records to your Kinesis Data Firehose delivery stream, and the trust policy enables Amazon SES to assume the role\.

This section provides examples of both policies\. For information about attaching policies to IAM roles, see [Modifying a Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the *IAM User Guide*\. 

### Permissions Policy<a name="event-publishing-add-event-destination-firehose-role-permission"></a>

The following permissions policy enables the role to publish data records to your Kinesis Data Firehose delivery stream\.

```
 1. {
 2.   "Version": "2012-10-17",
 3.   "Statement": [
 4.     {
 5.       "Sid": "",
 6.       "Effect": "Allow",
 7.       "Action": [
 8.         "firehose:PutRecordBatch"
 9.       ],
10.       "Resource": [
11.         "arn:aws:firehose:REGION:ACCOUNT-ID:deliverystream/DELIVERY-STREAM-NAME "
12.       ]
13.     }
14.   ]
15. }
```

### Trust Policy<a name="event-publishing-add-event-destination-firehose-role-trust"></a>

The following trust policy enables Amazon SES to assume the role\.

```
 1. {
 2.   "Version": "2012-10-17",
 3.   "Statement": [
 4.     {
 5.       "Sid": "",
 6.       "Effect": "Allow",
 7.       "Principal": {
 8.         "Service": "ses.amazonaws.com"
 9.       },
10.       "Action": "sts:AssumeRole",
11.       "Condition": {
12.         "StringEquals": {
13.           "sts:ExternalId": "ACCOUNT-ID"
14.         }
15.       }
16.     }
17.   ]
18. }
```