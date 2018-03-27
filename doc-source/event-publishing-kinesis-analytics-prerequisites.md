# Prerequisites<a name="event-publishing-kinesis-analytics-prerequisites"></a>

For this tutorial, you need the following:
+ **An AWS account** – To access any web service that AWS offers, you must first create an AWS account at [https://aws\.amazon\.com/](https://aws.amazon.com/)\.
+ **Verified email address** – To send emails using Amazon SES, you must verify your "From" address or domain to show that you own it\. If you are in the sandbox, you also must verify your "To" addresses\. You can verify email addresses or entire domains, but this tutorial requires a verified email address so that you can send an email from the Amazon SES console, which is the simplest way to send an email\. For information about how to verify an email address, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)\.
+ **Email application** – To use Amazon Kinesis Data Analytics as described in this tutorial, you must send a steady stream of emails through Amazon SES so that you generate a steady stream of email sending events\. This enables Amazon Kinesis Data Analytics to automatically detect the schema and then to process the event records with SQL\. Sending one email every ten seconds for five minutes is sufficient for this tutorial\. 
**Important**  
If you do not have an existing email campaign to send to real recipients, we strongly recommend that you send emails to an [Amazon SES mailbox simulator](mailbox-simulator.md) address\. Emails that you send to the mailbox simulator do not count toward your Amazon SES bounce and complaint rates or your daily sending quota\.

## Next Step<a name="event-publishing-kinesis-analytics-prerequisites-next-step"></a>

[Step 1: Create a Kinesis Firehose Delivery Stream](event-publishing-kinesis-analytics-firehose-stream.md)