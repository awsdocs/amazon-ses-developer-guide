# Automatically Pausing Email Sending<a name="monitoring-sender-reputation-pausing"></a>

To protect your sender reputation, you can temporarily pause email sending for messages sent using specific configuration sets, or for all messages sent from your Amazon SES account in a specific AWS Region\.

By using Amazon CloudWatch and Lambda, you can create a solution that automatically pauses your email sending when your reputation metrics \(such as bounce rate or complaint rate\) exceed certain thresholds\. This topic contains procedures for setting up this solution\.


+ [Automatically Pausing Email Sending for Your Amazon SES Account](monitoring-sender-reputation-pausing-account.md)
+ [Automatically Pausing Email Sending for a Configuration Set](monitoring-sender-reputation-pausing-configuration-set.md)