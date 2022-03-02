# Increasing your Amazon SES sending quotas<a name="manage-sending-quotas-request-increase"></a>

Your account has the following quotas per your current region that can be increased\.


| Resource | Default quota | Description | 
| --- | --- | --- | 
| Sending quota | 200 | Maximum number of emails that you can send in a 24\-hour period for this account in the current AWS Region\. | 
| Sending rate | 1 | Maximum number of emails that Amazon SES can accept each second for this account in the current AWS Region\. | 
| Maximum message size \(MB\) | 10 | The maximum message size that you can send\. This includes any images and attachments that are part of the email after MIME encoding\. For example, if you attach a 5MB file, the attachment size in the email after MIME encoding will be \~6\.85MB \(about 137% of the original file size\)\. | 

## Automatically increased sending quotas<a name="automatically-increased-sending-quotas"></a>

When your account is out of the sandbox and you're sending high\-quality production email, we might automatically increase the sending quotas for your account\. Often, we automatically increase these quotas before you actually need them to be increased\.

To qualify for automatic rate increases, all of the following statements have to be true:
+ **You send high\-quality content that your recipients want to receive** –Send content that recipients want and expect\. Stop sending email to customers who don't open your email\.
+ **You send actual production content** – Sending test messages to fake email addresses can have a negative effect on your bounce and complaint rates\. Also, sending messages only to internal recipients makes it difficult to determine if you're sending content that customers want to receive\. However, when you send your production messages to non\-internal recipients, we can accurately assess your email\-sending practices\.
+ **You send near your current quota** – To qualify for an automatic quota increase, your daily email volume should regularly approach the daily maximum for your account without exceeding it\.
+ **You have low bounce and complaint rates** – Minimize the number of bounces and complaints that you receive\. Having a high number of bounces and complaints can have a negative impact on your sending quotas\.

## User requested increased sending quotas<a name="user-requested-increased-sending-quotas"></a>

If your current sending quotas aren't adequate for your needs and we haven't automatically increased them, you can request an increase\. The following AWS resources can be use to submit the request depending on the type of sending quota you want to increase:
+ **Sending quota or Sending rate** – Increase requests for either of these can be submitted through the *AWS Service Quotas console* \(recommended for its more direct and easier process of requesting quota increases; however, we will continue to support these types of requests through Support Center until it is depreciated\)\. Choose the **AWS Service Quotas console** tab below for instructions\.
+ **Maximum message size** – Increase requests for this are currently only supported via *Support Center*\. Choose the **Support Center** tab below for instructions\. Amazon SES can accept messages larger than 10MB at a maximum rate of 40MB per second\. For example, if you send messages that are 20MB each, SES can accept those messages at a maximum rate of 2 messages per second\.

------
#### [ AWS Service Quotas console ]

**To request an increase on your Amazon SES sending quotas using the Service Quotas console\.**

1. Open the [Service Quotas console](https://console.aws.amazon.com/servicequotas/)\.

1. Select the region that you want the increase for by using the dropdown in the upper right\-hand corner of the console \(next to your account number\)\.

1.  In the navigation pane, choose **AWS services**\.

1. Choose **Amazon Simple Email Service \(SES\)**\.

1. Choose a quota, and follow the directions to request a quota increase\.

------
#### [ Support Center ]

**To request an increase on your Amazon SES sending quotas by opening a case in Support Center**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/](https://console.aws.amazon.com/)\.

1. On the **Support** menu, choose **Support Center**, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/dg/images/console_region_selector.png)

1. On the **My support cases** tab, choose **Create case**\.

1. Under **Create case**, choose **Service limit increase**\.

1. Under **Case classification**, complete the following sections:
   + For **Limit type**, choose **SES Service Limits**\.
   + For **Mail Type**, choose the type of email that you plan to send\. If more than one value applies, choose the option that applies to the majority of the email that you plan to send\.
   + For **Website URL**, enter the URL of your website\. Providing this information helps us better understand the type of content that you plan to send\.
   + For **My email sending complies with the AWS Service Terms and AUP**, choose the option that applies to your use case\.
   + For **I only send to recipients who have specifically requested my mail**, choose the option that applies to your use case\.
   + For **I have a process to handle bounces and complaints**, choose the option that applies to your use case\.

1. Under **Requests**, complete the following sections:
   + For **Region**, choose the AWS Region that your request applies to\.
   + For **Limit**, choose the type of quota increase that you want to request\. You can choose from the following options:
     + **Desired Maximum Send Quota** – Choose this option if you want to request an increase to the number of emails that your account can send per 24\-hour period in the selected AWS Region\. 

       **Desired Maximum Send Rate** – Choose this option if you want to request an increase to the number of emails that your account can send each second in the selected AWS Region\. 

       **Desired Maximum Message Size \(MB\), including attachments, for Email Sending** – Choose this option if you want to request an increase to the maximum message size \(not to exceed 40MB\) that your account can send in the selected AWS Region\.
   + For **New limit value**, enter the quota that you want to increase\. Only request the amount that you think you'll need\. Remember that you aren't guaranteed to receive the amount that you request\.
**Note**  
If you want to request both a sending *quota* increase and a sending *rate* increase, or if you want to request a sending quota increase in a different AWS Region, choose **Add another request**\. Then repeat this step\.

1. Under **Case Description**, for **Use case description**, describe how you plan to use Amazon SES to send email\. To help us process your request, answer the following questions:
   + How do you plan to build or acquire your mailing list?
   + How do you plan to handle bounces and complaints?
   + How can recipients opt out of receiving email from you?
   + How did you choose the new sending rate or sending quota that you specified in this request?
   + The following questions are only relevant to Desired Maximum Sending Message Size \(MB\) limit:
     + What is your use case for sending emails larger than 10MB in size?
     + How many messages larger than 10MB are you planning to send within a day?
     + What is the expected maximum sending TPS of messages larger than 10MB?
     + Please provide the daily sending distribution for messages larger than 10MB, i\.e\., will these emails be distributed across the day or sent at a specific time?

   If there's additional information that we should consider when evaluating your case, provide that information in this section as well\.

1. Under **Contact options**, for **Preferred contact language**, choose whether you want to receive communications for this case in **English** or **Japanese**\.

1. When you finish, choose **Submit**\.

------

**AWS Support team SLA for increase requests types**  
In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each request carefully\. If we're able to do so, we'll grant your request within the specified times listed below for the type of increase requested\. However, if we need to obtain additional information from you, it might take longer to resolve your request\. We reserve the right not to grant your request if your use case doesn't align with our policies\.  
**Sending quota or Sending rate**: Up to 24 hours\.
**Maximum message size**: Up to 14 days\.

**Note**  
While the Service Quotas console is available in many different languages, the actual support is only provided in English\. In addition to English, SES also provides support in Japanese via Support Center \(click the **Support Center** tab in the preceding section\)\. \.