# Step 7: Query Email Sending Events<a name="event-publishing-redshift-query"></a>

Now that you have generated some email sending events by sending emails with your configuration set and message tags, you can query those records in Amazon Redshift\.

**Note**  
We assume that SQL Workbench/J is currently open on your computer, and it is connected to your Amazon Redshift cluster, as described in [Step 2: Connect to Your Amazon Redshift Cluster](event-publishing-redshift-cluster-connect.md)\.

**To query email sending event data in Amazon Redshift from SQL Workbench/J**

1. To display all of your email sending records, copy the following query and paste it into the **Statement 1** window\.

   ```
   1. select * from ses;
   ```

1. Place the cursor within the statement \(somewhere before the semicolon\), and then choose the **Execute current statement** button\.

   You will see the email sending records for all of the emails you sent in [Step 6: Send Emails](event-publishing-redshift-send-email.md)\. The records in the following figure show that our `book` campaign had two complaints, and the `clothing` campaign had one bounce\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/dg/images/event_publishing_tutorial_redshift_all_records.png)

1. To count the `complaint` records for the campaign of type `book`, copy the following query and paste it into the **Statement 1** window\.

   ```
   1. select count(*) as numberOfComplaint from ses where event_type = 'Complaint' and campaign like '%book%';
   ```

1. Place the cursor within the statement \(somewhere before the semicolon\), and then choose the **Execute current statement** button\.

   The results are the following, showing that the book campaign had two complaints\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/dg/images/event_publishing_tutorial_redshift_complaint_count.png)