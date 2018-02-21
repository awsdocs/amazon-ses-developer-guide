# Publishing an MX Record for Amazon SES Email Receiving<a name="receiving-email-mx-record"></a>

A *mail exchanger record* \(*MX record* \) is a configuration that specifies which mail servers can accept email that is sent to your domain\.

To have Amazon SES manage your incoming email, you add an MX record to your domain's DNS configuration\. The MX record refers to the email receiving endpoint for the AWS Region where you use Amazon SES\. For example, the endpoint for the US West \(Oregon\) region is *inbound\-smtp\.us\-west\-2\.amazonaws\.com*\. See [Email Receiving Endpoints](regions.md#region-endpoints-receiving) for a complete list of endpoints\.

The procedures for publishing an MX record depend on your DNS or hosting provider\. See your provider's documentation for information about adding an MX record to the DNS configuration for your domain\. 