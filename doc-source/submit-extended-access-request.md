# Opening an SES Sending Limits Increase Case<a name="submit-extended-access-request"></a>

To apply for higher sending limits for Amazon SES, open a case in Support Center by using the following instructions\.

**To request higher sending limits**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Open an [SES Sending Limits Increase case](https://aws.amazon.com/ses/extendedaccessrequest/) in Support Center\. You can also access this link using the **Dedicated IPs** page in the Amazon SES console\.

1. In the form, provide the following information:
   + **Region** – Choose the AWS Region for which you are requesting a sending limit increase\. Your Amazon SES sandbox status and sending limits are separate for each AWS Region\. For more information, see [Regions and Amazon SES](regions.md)\.
   + **Limit** – Choose **Desired Daily Sending Quota** or **Desired Maximum Send Rate**\. Sending limits are described in [Managing Your Amazon SES Sending Limits](manage-sending-limits.md)\.
**Note**  
The rate at which Amazon SES accepts your messages might be less than the maximum send rate\.
   + **New limit value** – Enter the amount you are requesting\. Only request the amount that you think you'll need\. Remember that you are not guaranteed to receive the amount you request, and the higher the limit you request, the more justification you need to be considered for that amount\. 
   + **Mail Type** – Choose the type of email you plan to send using your dedicated IP address\. If multiple values apply, choose the type that will make up the majority of your email sending\.
   + **Website URL** – Type the URL of your website\. Providing this information will help us better understand the type of content you plan to send\.
   + **My email sending complies with the AWS Service Terms and AUP** – Choose the option that applies to your use case\.
   + **I only send to recipients who have specifically requested my mail** – Choose the option that applies to your use case\.
   + **I have a process to handle bounces and complaints** – Choose the option that applies to your use case\.
   + **Use Case Description** – Describe the ways in which you will use Amazon SES to send email\. To help us process your request more quickly, please answer the following questions:
     + How will you build or acquire your mailing list?
     + How will you handle bounces and complaints?
     + How can recipients unsubscribe from your mailing list, and how will you respond to those requests?
     + How did you choose the increase amount you specified in this request?

   When you finish, choose **Submit**\. We will respond to the case after reviewing your request\. Please allow one business day for processing\.