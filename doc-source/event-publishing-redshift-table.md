# Step 3: Create a Database Table<a name="event-publishing-redshift-table"></a>

After you connect to the initial database in Amazon Redshift, you typically use the initial database as the base for creating a new database\. However, in this simple tutorial, we create a table to hold your Amazon SES event publishing data directly within the initial database\.

For this tutorial, let's assume that we're interested in the following fields within the email sending event records\. All of these fields, except for `mail.tags.campaign`, are provided automatically by Amazon SES\. We introduce the `mail.tags.campaign` field when we send an email using `campaign` as a message tag in [Step 6: Send Emails](event-publishing-redshift-send-email.md)\.

+ `mail.messageId`

+ `eventType`

+ `mail.sendingAccountId`

+ `mail.timestamp`

+ `mail.destination`

+ `mail.tags.ses:configuration-set`

+ `mail.tags.campaign`

To access this information within your database, you must create a table\. The following procedure shows how to specify this information when you create the table in your database\.

**Note**  
We assume that SQL Workbench/J is currently open on your computer, and it is connected to your Amazon Redshift cluster, as described in previous step\.

**To create a table using SQL Workbench/J**

1. In SQL Workbench/J, copy the following code and paste it into the **Statement 1** window\.

   ```
   create table ses (
       message_id varchar(200) not null,
       event_type varchar(20) not null,
       sending_account_id char(12),
       timestamp varchar(50),
       destination text,
       configuration_set text,
       campaign text
   );
   ```

1. Place the cursor within the statement \(somewhere before the semicolon\), and then choose the **Execute current statement** button, as shown in the following figure\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_redshift_create_table.png)

1. In the **Messages** pane, verify that your table was successfully created\.

## Next Step<a name="event-publishing-redshift-table-next-step"></a>

[Step 4: Create a Kinesis Firehose Delivery Stream](event-publishing-redshift-firehose-stream.md)