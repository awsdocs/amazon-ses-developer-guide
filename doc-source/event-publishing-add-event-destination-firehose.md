# Set up a Kinesis Data Firehose event destination for Amazon SES event publishing<a name="event-publishing-add-event-destination-firehose"></a>

An Amazon Kinesis Data Firehose event destination represents an entity that publishes specific Amazon SES email sending events to Kinesis Data Firehose\. Because a Kinesis Data Firehose event destination exists within a configuration set only, you first have to [create a configuration set](event-publishing-create-configuration-set.md)\. Next, you add the event destination to the configuration set\.

The procedure in this section shows how to add Kinesis Data Firehose event destination details to a configuration set and assumes you have completed steps 1 through 6 in [Creating an event destination](event-destinations-manage.md#event-destination-add)\.

You can also use the [UpdateConfigurationSetEventDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateConfigurationSetEventDestination.html) operation in the Amazon SES API V2 destination to create and update event destinations\. 

**To add Kinesis Data Firehose event destination details to a configuration set using the console**

1. These are the detailed instructions for selecting Kinesis Data Firehose as your event destination type in [Step 7](event-destinations-manage.md#specify-event-dest-step) and assumes you have completed all the previous steps in [Creating an event destination](event-destinations-manage.md#event-destination-add)\. After selecting the Kinesis Data Firehose **Destination type** and enabling **Event publishing**, the **Amazon Kinesis Data Firehose delivery stream** panel will appear \- its fields are addressed in the following steps\.

1. For **Delivery stream**, choose an existing Kinesis Data Firehose delivery stream, or choose **Create new stream** to create a new one using the Kinesis Data Firehose console\.

   For information about creating a stream using the Kinesis Data Firehose console, see [Creating an Amazon Kinesis Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.

1. For **Identity and Access Management \(IAM\) Role**, choose an IAM role for which Amazon SES has permission to publish to Kinesis Data Firehose on your behalf\. You can choose an existing role, have Amazon SES create a role for you, or create your own role\.

   If you choose an existing role or create your own role, you must manually modify the role's policies to give the role permission to access the Kinesis Data Firehose delivery stream, and to give Amazon SES permission to assume the role\. For example policies, see [Giving Amazon SES Permission to Publish to Your Kinesis Data Firehose Delivery Stream](#event-publishing-add-event-destination-firehose-role)\. 

1. Choose **Next**\.

1. On the review screen, if you're satisfied with how you defined your event destination, choose **Add destination**\.

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
11.         "arn:aws:firehose:delivery-region:111122223333:deliverystream/delivery-stream-name"              
12.       ]
13.     }
14.   ]
15. }
```

Make the following changes to the preceding policy example:
+ Replace *delivery\-region* with the AWS Region where you created the Kinesis Data Firehose delivery stream\.
+ Replace *111122223333* with your AWS account ID\.
+ Replace *delivery\-stream\-name* with the name of the Kinesis Data Firehose delivery stream\.

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
13.           "AWS:SourceAccount": "111122223333",
14.           "AWS:SourceArn": "arn:aws:ses:delivery-region:111122223333:configuration-set/configuration-set-name"
15.         }
16.       }
17.     }
18.   ]
19. }
```

Make the following changes to the preceding policy example:
+ Replace *delivery\-region* with the AWS Region where you created the Kinesis Data Firehose delivery stream\.
+ Replace *111122223333* with your AWS account ID\.
+ Replace *configuration\-set\-name* with the name of your configuration set associated with the Kinesis Data Firehose delivery stream\.