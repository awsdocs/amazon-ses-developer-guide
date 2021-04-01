# Using list management<a name="sending-email-list-management"></a>

Amazon SES offers list management capabilities, which means customers can manage their own mailing lists, known as contact lists\. A *contact list* is a list that allows you to store all of your contacts that have subscribed to a particular topic or topics\. A *contact* is an end\-user who is receiving your emails\. A *topic* is an interest group, theme, or label within a list\. Lists can have multiple topics\.

By using the [ListContacts](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_ListContacts.html) operation in the Amazon SES API v2, you can retrieve a list of all your contacts who have subscribed to a particular topic, to whom you can send emails using the [SendEmail](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_SendEmail.html) operation\.

You can manually add or remove individual or bulk addresses from the account\-level suppression list by using the Amazon SES API v2\.

**Note**  
To bulk add or remove addresses, you must have production access\. To learn more about the sandbox, see [Moving out of the Amazon SES sandbox](request-production-access.md)\.

For information about subscription management, see [Using subscription management](sending-email-subscription-management.md)\.

## List management overview<a name="list-management-overview"></a>

You should consider the following factors when you use list management:
+ You can specify list topics while creating the list\.
+ Only one contact list is allowed per AWS account\.
+ A list can have a maximum of 20 topics\.
+ You can update an existing contact list, including adding new topics to the list, adding or deleting contacts from a list, and updating contact preferences for a list or topic\.
+ You can update topic metadata, such as the topic display name or description\.
+ You can get a list of contacts in a contact list, contacts subscribed to a topic, contacts unsubscribed from a topic, and contacts unsubscribed from all topics in the list\.
+ You can import your existing contact lists to Amazon SES using the [CreateImportJob](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateImportJob.html) API\.
+ Amazon SES will bounce an email if it is sent to an unsubscribed contact on your contact list\. For more information, see [Using subscription management](sending-email-subscription-management.md)\.
+ Each contact can have associated attributes which you can use to store information about that contact\.

## Configuring list management<a name="configuring-list-management"></a>

You can use the following operations to configure list management capabilities\. For the full list of contact list and contact operations, see the [Amazon SES API v2 Reference](https://docs.aws.amazon.com/ses/latest/APIReference-V2/Welcome.html)\.

### Create a contact list<a name="configuring-list-management-create-contact-list"></a>

You can use the [CreateContactList](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateContactList.html) operation in the Amazon SES API v2 to create a contact list\. You can quickly and easily configure this setting by using the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To create a contact list by using the AWS CLI**
+ At the command line, enter the following command:

  ```
  aws sesv2 create-contact-list --cli-input-json file://CONTACT-LIST-JSON
  ```

  In the preceding command, replace *CONTACT\-LIST\-JSON* with the path to your JSON file for your [CreateContactList](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateContactList.html) request\.

  An example CreateContactList input JSON file for the request is as follows:

  ```
  {
      "ContactListName": "ExampleContactListName",
      "Description": "Creating a contact list example",
      "Topics": [
       {
           "TopicName": "Sports",
           "DisplayName": "Sports Newsletter",
           "Description": "Sign up for our free newsletter to receive updates on all sports.",
           "DefaultSubscriptionStatus": "OPT_OUT"
       },
       {
           "TopicName": "Cycling",
           "DisplayName": "Cycling newsletter",
           "Description": "Never miss a cycling update by subscribing to our newsletter.",
           "DefaultSubscriptionStatus": "OPT_IN"
       },
       {
           "TopicName": "NewProducts",
           "DisplayName": "New products",
           "Description": "Hear about new products by subscribing to this mailing list.",
           "DefaultSubscriptionStatus": "OPT_IN"
       },
       {
           "TopicName": "DailyUpdates",
           "DisplayName": "Daily updates",
           "Description": "Start your day with sport updates, Monday through Friday.",
           "DefaultSubscriptionStatus": "OPT_OUT"
       }
      ]
  }
  ```

### Create a contact<a name="configuring-list-management-create-contact"></a>

You can use the [CreateContact](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateContact.html) operation in the Amazon SES API v2 to create a contact\. You can quickly and easily configure this setting by using the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To create a contact by using the AWS CLI**
+ At the command line, enter the following command:

  ```
  aws sesv2 create-contact --cli-input-json file://CONTACT-JSON
  ```

  In the preceding command, replace *CONTACT\-JSON* with the path to your JSON file for your [CreateContact](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateContact.html) request\.

  An example CreateContact input JSON file for the request is as follows:

  ```
  {
      "ContactListName": "ExampleContactListName",
      "EmailAddress": "example@amazon.com",
      "UnsubscribeAll": false,
      "TopicPreferences": [
          {
              "TopicName": "Sports",
              "SubscriptionStatus": "OPT_IN"
          }
      ],
      "AttributesData": "{\"Name\": \"John\", \"Location\": \"Seattle\"}"
  }
  ```

  In the example above, an UnsubscribeAll value of `false` shows that the contact has not unsubscribed from all topics, where a value of `true` would mean the contact has unsubscribed from all topics\.

  TopicPreferences includes information about the contact’s subscription status to topics\. In the preceding example, the contact has opted in to the "Sports" topic and will receive all emails to the "Sports" topic\.

  The AttributesData is a JSON field where you can put any metadata about our contact\. It must be a valid JSON object\.

### Bulk importing contacts to your contact list<a name="configuring-list-management-bulk-import"></a>

You can manually add addresses in bulk by first uploading your contacts into an Amazon S3 object that you have permission to access followed by using the [CreateImportJob](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateImportJob.html) operation in the Amazon SES API v2\.

You should create a contact list before importing your contacts\.

**Note**  
You can add up to 1 million contacts to a contact list per ImportJob\.

To add contacts in bulk to your contact list, complete the following steps\.
+ Upload your contacts into an Amazon S3 object in either CSV or JSON format\.

  **CSV format**

  The first line of the file that is uploaded to Amazon S3 should be a header line\.

  The topicPreferences object needs to be flattened for the CSV format\. Every topic in the topicPreferences will have a separate header field\.

  CSV format example for adding contacts in bulk to a contact list:

  ```
  emailAddress,unsubscribeAll,attributesData,topicPreferences.Sports,topicPreferences.Cycling
  example1@amazon.com,false,{"Name": "John"},OPT_IN,OPT_OUT
  example2@amazon.com,true,,OPT_OUT,OPT_OUT
  ```

  **JSON format**

  Only newline\-delimited JSON files are supported\. In this format, each line is a complete JSON object that contains one contact's information\.

  JSON format example for adding contacts in bulk to a contact list:

  ```
  {
       "emailAddress": "example1@amazon.com",
       "unsusbcribeAll": false,
       "attributesData": "{\"Name\":\"John\"}",
       "topicPreferences": [
        {
            "topicName": "Sports",
            "subscriptionStatus": "OPT_IN"
        },
        {
            "topicName": "Cycling",
            "subscriptionStatus": "OPT_OUT"
        }
       ]
  }
  {
       "emailAddress": "example2@amazon.com",
       "unsusbcribeAll": true,
       "topicPreferences": [
        {
            "topicName": "Sports",
            "subscriptionStatus": "OPT_OUT"
        },
        {
            "topicName": "Cycling",
            "subscriptionStatus": "OPT_OUT"
        }
       ]
  }
  ```

  In the preceding examples, replace *example1@amazon\.com* and *example2@amazon\.com* with the email addresses you want to add to the contact list\. Replace the attributesData values with the values specific to the contact\. Additionally, replace *Sports* and *Cycling* with the topicName that applies to your contact\. The acceptable topicPreferences are *OPT\_IN* and *OPT\_OUT*\.

  The following attributes are supported when uploading your contacts into an Amazon S3 object in either CSV or JSON format:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-email-list-management.html)
+ Use the [CreateImportJob](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateImportJob.html) operation in the Amazon SES API v2\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To manually add contacts in bulk to your contact list by using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 create-import-job \
  --import-destination "{\"ContactListDestination\": {\"ContactListName\”:\”ExampleContactListName\", \"ContactListImportAction\”:\”PUT\"}}" \
  --import-data-source "{\"S3Url\": \"s3://s3bucket/s3object\",\"DataFormat\": \"CSV\"}"
  ```

------
#### [ Windows ]

  ```
  aws sesv2 create-import-job `
  --import-destination "{\"ContacListDestination\": {\"ContactListName\”:\”ExampleContactListName\", \"ContactListImportAction\”:\”PUT\"}}" `
  --import-data-source "{\"S3Url\": \"s3://s3bucket/s3object\",\"DataFormat\": \"CSV\"}"
  ```

------

  In the preceding examples, replace *s3bucket* and *s3object* with the Amazon S3 bucket name and Amazon S3 object name\.

### List your contacts to send them emails<a name="configuring-list-management-list-contacts"></a>

You can use the [ListContacts](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_ListContacts.html) operation to retrieve a list of all your contacts who have subscribed to a particular topic, in conjunction with the [SendEmail](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_SendEmail.html) operation, which allows you to send them emails\.

**To list contacts by using the AWS CLI**

1. At the command line, enter the following command:

   ```
   aws sesv2 list-contacts --cli-input-json file://LIST-CONTACTS-JSON
   ```

   In the preceding command, replace *LIST\-CONTACTS\-JSON* with the path to your JSON file for your [ListContacts](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_ListContacts.html) request\.

   An example ListContacts input JSON file for the request is as follows:

   ```
   {
       "ContactListName": "ExampleContactListName",
       "Filter": {
           "FilteredStatus": "OPT_IN",
           "TopicFilter": {
               "TopicName": "Cycling",
               "UseDefaultIfPreferenceUnavailable": true
           }
       },
       "PageSize": 50
   }
   ```

   The FilteredStatus shows the subscription status for which you want to filter, which is either `OPT_IN` or `OPT_OUT`\.

   The TopicFilter is an optional filter which specifies which topic you want results for, and in the example above, that is "Cycling\."

   UseDefaultIfPreferenceUnavailable can have a value of `true` or `false`\. If `true`, the topic default preference will be used if the contact doesn’t have any explicit preference for a topic\. If `false`, only contacts with an explicitly set preference are considered for filtering\.

1. After listing the contacts in your list using the above [ListContacts](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_ListContacts.html) operation, use the SendEmail operation to send emails to each of your contacts\. When using the [SendEmail](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_SendEmail.html) operation, you can specify ListManagementOptions to enable Amazon SES to add unsubscribe links to your email\. For more information, see [Using subscription management](sending-email-subscription-management.md)\.

   If you include ListManagementOptions in your SendEmail request to a recipient email address that is not on your contact list, then a contact will be created on your list automatically\. 

   Amazon SES will bounce an email if it is sent to an unsubscribed contact on your contact list, which means you won’t need to update your SendEmail requests to avoid sending to contacts who have unsubscribed\.

   To use the SendEmail operation, include the contactListName and topicName to which the email belongs, as follows\. The topicName is optional\.

   ```
   ListManagementOptions:
       String contactListName
       String topicName
   ```

1. Alternatively, you can use the `X-SES-LIST-MANAGEMENT-OPTIONS` header to specify a list and topic name while sending email using SMTP interface\.

   To specify a list and topic name while sending email using the SMTP interface, add the following email header to your message:

   `X-SES-LIST-MANAGEMENT-OPTIONS: {contactListName}; topic={topicName}`
