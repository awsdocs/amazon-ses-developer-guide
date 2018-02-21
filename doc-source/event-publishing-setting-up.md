# Setting Up Amazon SES Event Publishing<a name="event-publishing-setting-up"></a>

This section describes what you need to do to configure Amazon SES to publish your email sending events to Amazon CloudWatch or Amazon Kinesis Firehose\.

You first create a *configuration set* using the Amazon SES console or API\. After you create a configuration set, you add one or more *event destinations* \(CloudWatch or Kinesis Firehose\) to the configuration set, and configure parameters unique to the event destination\. Then, each time you send an email, you include the configuration set name and email characteristics, called *message tags*, as parameters to the API, or Amazon SES\-specific headers in the email\.

These steps are explained in the following topics\.

1. [Step 1: Create a Configuration Set](event-publishing-create-configuration-set.md)

1. [Add an Event Destination](event-publishing-add-event-destination.md)

1. [Step 3: Send Email](event-publishing-send-email.md)