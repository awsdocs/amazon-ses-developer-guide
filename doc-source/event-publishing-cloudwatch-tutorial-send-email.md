# Step 2: Send Emails<a name="event-publishing-cloudwatch-tutorial-send-email"></a>

For Amazon SES to publish events associated with an email, you must specify a configuration set when you send the email\. You can also include message tags to categorize the email\. This section shows how to send a simple email that specifies a configuration set and message tags using the Amazon SES console\. You send the email to the Amazon SES mailbox simulator so that you can test bounces, complaints, and other email sending outcomes\.

**To send an email using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the **Navigation** pane of the Amazon SES console, under **Identity Management**, choose **Email Addresses**\.

1. In the list of identities, select the check box of an email address that you have successfully [verified with Amazon SES](verify-email-addresses.md)\.

1. Choose **Send a Test Email**\.

1. In the **Send Test Email** dialog box, for **Email Format**, choose **Raw**\.

1. For the **To** address, type an address from the [Amazon SES mailbox simulator](send-email-simulator.md), such as `complaint@simulator.amazonses.com` or `bounce@simulator.amazonses.com`\.

1. Copy and paste the following message in its entirety into the **Message** text box, replacing `CONFIGURATION-SET-NAME` with the name of the configuration set you created in [Step 1: Set up a Configuration Set](event-publishing-cloudwatch-tutorial-configuration-set.md), and replacing `FROM-ADDRESS` with the verified address you are sending this email from\.

   ```
   1. X-SES-MESSAGE-TAGS: campaign=book
   2. X-SES-CONFIGURATION-SET: CONFIGURATION-SET-NAME
   3. Subject: Amazon SES Event Publishing Test
   4. From: Amazon SES User <FROM-ADDRESS>
   5. MIME-Version: 1.0
   6. Content-Type: text/plain
   7. 
   8. This is a test message.
   ```

1. Choose **Send Test Email**\.

1. Repeat this procedure a few times so that you generate multiple email sending events\. For a few of the emails, change the value of the `campaign` message tag to `clothing` to simulate sending for a different email campaign\.

## Next Step<a name="event-publishing-cloudwatch-tutorial-send-email-next-step"></a>

 [Step 3: Graph Email Sending Events](event-publishing-cloudwatch-tutorial-graph.md) 
