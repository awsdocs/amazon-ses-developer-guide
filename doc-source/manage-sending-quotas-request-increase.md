# Increasing your Amazon SES sending quotas<a name="manage-sending-quotas-request-increase"></a>

Your account has the following quotas per your current region that can be increased\.


| Resource | Default quota | Description | 
| --- | --- | --- | 
| Sending quota | 200 | Maximum number of emails that you can send in a 24\-hour period for this account in the current AWS Region\. | 
| Sending rate | 1 | Maximum number of emails that Amazon SES can accept each second for this account in the current AWS Region\. | 

## Automatically increased sending quotas<a name="automatically-increased-sending-quotas"></a>

When your account is out of the sandbox and you're sending high\-quality production email, we might automatically increase the sending quotas for your account\. Often, we automatically increase these quotas before you actually need them to be increased\.

To qualify for automatic rate increases, all of the following statements have to be true:
+ **You send high\-quality content that your recipients want to receive** –Send content that recipients want and expect\. Stop sending email to customers who don't open your email\.
+ **You send actual production content** – Sending test messages to fake email addresses can have a negative effect on your bounce and complaint rates\. Also, sending messages only to internal recipients makes it difficult to determine if you're sending content that customers want to receive\. However, when you send your production messages to non\-internal recipients, we can accurately assess your email\-sending practices\.
+ **You send near your current quota** – To qualify for an automatic quota increase, your daily email volume should regularly approach the daily maximum for your account without exceeding it\.
+ **You have low bounce and complaint rates** – Minimize the number of bounces and complaints that you receive\. Having a high number of bounces and complaints can have a negative impact on your sending quotas\.

## User requested increased sending quotas<a name="user-requested-increased-sending-quotas"></a>

If your current sending quotas aren't adequate for your needs and we haven't automatically increased them, you can request an increase: 
+ **Sending quota or Sending rate** – Increase requests for either of these can be submitted through the *AWS Service Quotas console*\. 

**To request an increase on your Amazon SES sending quotas using the Service Quotas console\.**

1. Open the [Service Quotas console](https://console.aws.amazon.com/servicequotas/)\.

1. Select the region that you want the increase for by using the dropdown in the upper right\-hand corner of the console \(next to your account number\)\.

1.  In the navigation pane, choose **AWS services**\.

1. Choose **Amazon Simple Email Service \(SES\)**\.

1. Choose a quota, and follow the directions to request a quota increase\.

**AWS Support team SLA for increase requests types**  
In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each request carefully\. If we're able to do so, we'll grant your request within the specified times listed below for the type of increase requested\. However, if we need to obtain additional information from you, it might take longer to resolve your request\. We reserve the right not to grant your request if your use case doesn't align with our policies\.  
**Sending quota or Sending rate**: Up to 24 hours\.

**Note**  
While the Service Quotas console is available in many different languages, the actual support is only provided in English\.  