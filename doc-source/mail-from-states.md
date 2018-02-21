# MAIL FROM Domain Setup States with Amazon SES<a name="mail-from-states"></a>

After you configure an identity to use a custom MAIL FROM domain, the state of the setup is "pending" while Amazon SES attempts to detect the required MX record in your DNS settings\. The state then changes depending on whether Amazon SES detects the MX record\. The following table describes the email\-sending behavior, and the Amazon SES actions associated with each state\. Each time the state changes, Amazon SES sends a notification to the email address associated with your AWS account\.


****  

| State | Email Sending Behavior | Amazon SES Actions | 
| --- | --- | --- | 
|  Pending  |  Uses custom MAIL FROM fallback setting  |  Amazon SES attempts to detect the required MX record for 72 hours\. If unsuccessful, the state changes to "Failed"\.  | 
|  Success  |  Uses custom MAIL FROM domain  |  Amazon SES continuously checks that the required MX record is in place\.   | 
|  TemporaryFailure  |  Uses custom MAIL FROM fallback setting  |  Amazon SES attempts to detect the required MX record for 72 hours\. If unsuccessful, the state changes to "Failed"; if successful, the state changes to "Success"\.  | 
|  Failed  |  Uses custom MAIL FROM fallback setting  |  Amazon SES no longer attempts to detect the required MX record\. To use a custom MAIL FROM domain, you must restart the setup process in [Setting a MAIL FROM Domain](mail-from-set.md)\.   | 