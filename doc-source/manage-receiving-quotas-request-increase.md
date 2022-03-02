# Increasing your Amazon SES receiving quotas<a name="manage-receiving-quotas-request-increase"></a>

Your account has the following receiving quota per your current region that can be increased\.


| Resource | Default quota | Description | 
| --- | --- | --- | 
| Maximum message size \(MB\) | 30 | The maximum message size that can be sent to your identity and stored in an Amazon S3 bucket\. | 

## User requested increased receiving quotas<a name="user-requested-increased-receiving-quotas"></a>

If your current receiving quota isn't adequate for your needs, you can request an increase\.

------
#### [ AWS Service Quotas console ]

**Note**  
Maximum message size increase requests for email receiving are currently only supported via Support Center\. Please select the **Support Center** tab above to make your request\.

------
#### [ Support Center ]

**To request an increase on your Amazon SES receiving quotas by opening a case in Support Center**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/](https://console.aws.amazon.com/)\.

1. On the **Support** menu, choose **Support Center**, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/dg/images/console_region_selector.png)

1. On the **My support cases** tab, choose **Create case**\.

1. Under **Create case**, choose **Service limit increase**\.

1. Under **Case classification**, complete the following sections:
   + For **Limit type**, choose **SES Service Limits**\.

1. Under **Requests**, complete the following sections:
   + For **Region**, choose the AWS Region that your request applies to\.
   + For **Limit**, choose **Desired Maximum Message Size \(MB\), including attachments, for Email Receiving** – Choose this option if you want to request an increase to the maximum message size \(not to exceed 40MB\) that can be sent to your identity and stored in an Amazon S3 bucket in the selected AWS Region\.
   + For **New limit value**, enter the quota that you want to increase\. Only request the amount that you think you'll need\. Remember that you aren't guaranteed to receive the amount that you request\.

1. Under **Case Description**, for **Use case description**, describe how you plan to use Amazon SES to receive email\. To help us process your request, answer the following questions:
   + What is your use case for receiving emails larger than 30MB in size?
   + How many messages larger than 30MB are you planning to receive within a day?
   + What is the expected maximum receiving TPS of messages larger than 30MB?
   + Please provide the daily email receiving distribution for messages larger than 30MB, i\.e\., will these emails be received throughout the day or at a specific time?

   If there's additional information that we should consider when evaluating your case, provide that information in this section as well\.

1. Under **Contact options**, for **Preferred contact language**, choose whether you want to receive communications for this case in **English** or **Japanese**\.

1. When you finish, choose **Submit**\.

While the AWS Support team provides an initial response to your request within 24 hours, for maximum message size increase requests, it can take up to 14 days to process\.

We might not be able to grant your request if your use case doesn't align with our policies\.

------

**Note**  
While the Service Quotas console is available in many different languages, the actual support is only provided in English\. In addition to English, SES also provides support in Japanese via Support Center \(click the **Support Center** tab in the preceding section\)\. \.