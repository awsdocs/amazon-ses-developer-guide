# Opening a Case to Increase Amazon SES Sending Quotas<a name="manage-sending-quotas-request-increase-procedure"></a>

To apply for higher sending quotas for Amazon SES, open a case in Support Center by completing the following steps\.

**To request higher sending quotas**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/](https://console.aws.amazon.com/)\.

1. On the **Support** menu, choose **Support Center**, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/console_region_selector.png)

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
     + **Desired Maximum Send Quota** – Choose this option if you want to request an increase to the number of emails that your account can send per 24\-hour period in the selected Region\. 

       **Desired Maximum Send Rate** – Choose this option if you want to request an increase to the number of emails that your account can send each second in the selected Region\. 
   + For **New limit value**, enter the quota that you want to increase\. Only request the amount that you think you'll need\. Remember that you aren't guaranteed to receive the amount that you request\.
**Note**  
If you want to request both a sending *quota* increase and a sending *rate* increase, or if you want to request a sending quota increase in a different AWS Region, choose **Add another request**\. Then repeat this step\.

1. Under **Case Description**, for **Use case description**, describe how you plan to use Amazon SES to send email\. To help us process your request, answer the following questions:
   + How do you plan to build or acquire your mailing list?
   + How do you plan to handle bounces and complaints?
   + How can recipients opt out of receiving email from you?
   + How did you choose the new sending rate or sending quota that you specified in this request?

   If there's additional information that we should consider when evaluating your case, provide that information in this section as well\.

1. Under **Contact options**, for **Preferred contact language**, choose whether you want to receive communications for this case in **English** or **Japanese**\.

1. When you finish, choose **Submit**\.

The AWS Support team provides an initial response to your request within 24 hours\.

In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each request carefully\. If we're able to do so, we'll grant your request within this 24\-hour period\. However, if we need to obtain additional information from you, it might take longer to resolve your request\.

We might not be able to grant your request if your use case doesn't align with our policies\.