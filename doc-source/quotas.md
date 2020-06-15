# Service quotas in Amazon SES<a name="quotas"></a>

The following sections list and describe the quotas that apply to Amazon SES resources and operations\. Some quotas can be increased, while others can't\. To determine whether you can request an increase for a quota, refer to the **Eligible for Increase** column in each section\. 

## Email sending quotas<a name="quotas-email-sending"></a>

The following quotas apply to sending email through Amazon SES\.

### Sending quotas<a name="quotas-sending"></a>

**Note**  
Quotas are based on the number of recipients, rather than on the number of messages\.


****  

| Resource | Default Quota | Eligible for Increase? | 
| --- | --- | --- | 
| Number of emails that can be sent per 24\-hour period |  If your account is in the sandbox, you can send up to 200 emails per 24\-hour period\. If your account is out of the sandbox, this number varies based on your specific use case\.  This value was referred to in the past as your "sending quota\."   |  [Yes](manage-sending-quotas-request-increase.md)  | 
| Number of emails that can be sent per second \(sending rate\) |  If your account is in the sandbox, you can send 1 email per second\. If your account is out of the sandbox, this rate varies based on your specific use case\.  |  [Yes](manage-sending-quotas-request-increase.md)  | 

### Message quotas<a name="quotas-message"></a>


****  

| Resource | Default Quota | Eligible for Increase? | 
| --- | --- | --- | 
|  Maximum message size \(including attachments\)  |  10 MB per message \(after base64 encoding\)\.  |  No  | 

### Sender and recipient quotas<a name="quotas-sender-recipient"></a>


****  

| Resource | Default Quota | Eligible for Increase? | 
| --- | --- | --- | 
|  Maximum number of recipients per message  |  50 recipients per message\.  A recipient is any "To", "CC", or "BCC" address\.   |  No  | 
|  Maximum number of identities that you can verify  |  10,000 identities per AWS Region\.  An *identity* is a domain or email address that you use to send email through Amazon SES\.   |  No  | 

### Quotas related to event publishing<a name="quotas-publishing"></a>


****  

| Resource | Default Quota | Eligible for Increase? | 
| --- | --- | --- | 
|  Maximum number of configuration sets  |  10,000  |  No  | 
|  Maximum length of configuration set name  |  Configuration set names can contain up to 64 alphanumeric characters\. They can also contain hyphens \(\-\) and underscores \(\_\)\. Names can't contain spaces, accented characters, or any other special characters\.  |  No  | 
|  Maximum number of event destinations per configuration set  |  10  |  No  | 
|  Maximum number of dimensions per CloudWatch event destination  |  10  |  No  | 

### Email template quotas<a name="quotas-templates"></a>


****  

| Resource | Default Quota | Eligible for Increase? | 
| --- | --- | --- | 
|  Maximum number of email templates in each AWS Region  |  10,000  |  No  | 
|  Maximum template size  |  500 KB  |  No  | 
|  Maximum number of replacement values in each template  |  Unlimited  |  N/A  | 
| Maximum number of recipients for each templated email | 50 destinations\. A *destination* is any email address on the "To", "CC", or "BCC" lines\.  The number of destinations you can contact in a single call to the API may be limited by your account's maximum sending rate\.   |  No  | 

## Quotas related to email receiving<a name="quotas-email-receiving"></a>

The following table lists the quotas associated with receiving email through Amazon SES\.


****  

| Resource | Default Quota | Eligible for Increase? | 
| --- | --- | --- | 
|  Maximum number of rules per receipt rule set  |  200  |  No  | 
|  Maximum number of actions per receipt rule  |  10  |  No  | 
|  Maximum number of recipients per receipt rule  |  100  |  No  | 
|  Maximum number of receipt rule sets per AWS account  |  40  |  No  | 
|  Maximum number of IP address filters per AWS account  |  100  |  No  | 
|  Maximum email size \(including headers\) that can be stored in an Amazon S3 bucket  |  30 MB  |  No  | 
|  Maximum email size \(including headers\) that can be published using an Amazon SNS notification  |  150 KB  |  No  | 

## General quotas<a name="quotas-email-general"></a>

The following table lists quotas that apply to both sending and receiving email through Amazon SES\.

### Amazon SES API quotas<a name="quotas-api"></a>


****  

| Resource | Default Quota | Eligible for Increase? | 
| --- | --- | --- | 
|  Rate at which you can call Amazon SES API actions  |  All actions \(except for `SendEmail` and `SendRawEmail`\) are throttled at one request per second\. For more information about the Amazon SES API, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\.  |  No  | 


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 