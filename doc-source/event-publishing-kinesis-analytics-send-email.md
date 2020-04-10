# Step 3: Send Emails<a name="event-publishing-kinesis-analytics-send-email"></a>

Because this tutorial uses the Amazon Kinesis Data Analytics console to process and analyze streaming data, you must set up a steady stream of emails through Amazon SES\. This tutorial assumes that you have an application that can send these emails\. Sending one email every ten seconds for five minutes is sufficient for this tutorial\. We highly recommend that you use a "To" address from the [Amazon SES mailbox simulator](send-email-simulator.md), such as `success@simulator.amazonses.com`\.

To enable event publishing for an email, you provide the name of the configuration set to Amazon SES when you send the email\. You can optionally include message tags to categorize the email\. You provide this information to Amazon SES as either parameters to the email sending API, Amazon SES\-specific email headers, or custom headers in your MIME message\. For more information, see [Send Email Using Amazon SES Event Publishing](event-publishing-send-email.md)\.

For example, you might add the following Amazon SES\-specific email headers to your email to simulate a book campaign\. Replace `CONFIGURATION-SET-NAME` with the name of the configuration set you created in [Step 2: Set up a Configuration Set](event-publishing-kinesis-analytics-configuration-set.md)\.

```
1. X-SES-CONFIGURATION-SET: CONFIGURATION-SET-NAME
2. X-SES-MESSAGE-TAGS: campaign=book
```

## Next Step<a name="event-publishing-kinesis-analytics-send-email-next-step"></a>

[Step 4: Create an Amazon Kinesis Data Analytics Application](event-publishing-kinesis-analytics-application.md)