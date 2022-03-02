# Requesting and relinquishing dedicated IP addresses<a name="dedicated-ip-case"></a>

This section describes how to request and relinquish dedicated IP addresses by submitting a request in the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\. We charge your account an additional monthly fee for each dedicated IP address that you lease for use with Amazon SES\. For more information about the costs associated with dedicated IP addresses, see [Amazon SES Pricing](https://aws.amazon.com/ses/pricing/#Optional_Services)\.

## Best Practices for Working with Dedicated IP Addresses<a name="dedicated-ip-case-best-practices"></a>

Although there's no minimum commitment, we recommend that you lease more than one dedicated IP address in each AWS Region where you use Amazon SES\. Each AWS Region consists of multiple physical locations, called *Availability Zones*\. When you lease more than one dedicated IP address, we distribute those addresses as evenly as possible across the Availability Zones in the AWS Region that you specified in your request\. Distributing your dedicated IP addresses across Availability Zones in this way increases the availability and redundancy of your dedicated IP addresses\.

For a list of all of the Regions where Amazon SES is currently available, see [AWS Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *Amazon Web Services General Reference*\. To learn more about the number of Availability Zones that are available in each Region, see [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)\.

## Request dedicated IP addresses<a name="dedicated-ip-case-request"></a>

The following steps show how to request dedicated IP addresses by creating a service quota increase case in the AWS Support Center\. You can use this process to request as many dedicated IP addresses as you need\.

**To request dedicated IP addresses**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane on the left side of the screen, choose **Configuration**, then choose **Dedicated IPs**\.

1. To open a new case in the Support Center, choose **Request or relinquish dedicated IPs** at the top of the page\.

1. Under **Case details**, complete the following sections:
   + For **Limit type**, keep **SES Sending Limits**\.
   + For **Mail Type**, choose the type of email that you plan to send using your dedicated IP address\. If multiple values apply, choose the option that applies to the majority of the email that you plan to send\.
   + For **Website URL**, enter the URL of your website\. Providing this information helps us better understand the type of content that you plan to send\.
   + For **Describe, in detail, how you will only send to recipients who have specifically requested your mail **, provide an optional response consistent with your use case\.
   + For **Describe, in detail, the process that you will follow when you receive bounce and complaint notifications **, provide an optional response consistent with your use case\.
   + For **Will you comply with AWS Service Terms and AUP**, choose the option that applies to your use case\.

1. Under **Requests**, complete the following sections:
   + For **Region**, choose the AWS Region that your request applies to\.
   + For **Limit**, choose **Desired Maximum Send Rate**\.
   + For **New limit value**, enter the maximum number of messages that you want to send per second\. We use the value that you provide to calculate the number of dedicated IP addresses that you need to implement your use case\. For this reason, the estimate that you provide should be as accurate as possible\.
**Note**  
A single, dedicated IP address can only be used in the AWS Region that you chose in this step\. If you want to request dedicated IP addresses for use in another AWS Region, choose **Add another request**, and then complete the **Region**, **Limit**, and **New limit value** fields for the additional Region\. Repeat this process for each Region that you want to use dedicated IP addresses in\.

1. Under **Case description**, for **Use case description**, state that you want to request dedicated IP addresses\. If you want to request a specific number of dedicated IP addresses, mention that as well\. If you don't specify a number of dedicated IP addresses, we'll provide the number of dedicated IP addresses that are necessary to meet the sending rate requirement that you specified in the previous step\.

   Next, describe how you plan to use dedicated IP addresses to send email using Amazon SES\. Include information about why you want to use dedicated IP addresses instead of shared IP addresses\. This information helps us better understand your use case\.

1. Under **Contact options**, for **Preferred contact language**, choose whether you want to receive communications for this case in **English** or **Japanese**\.

1. When you finish, choose **Submit**\.

After you submit the form, we evaluate your request\. If we grant your request, we reply to your case in Support Center to confirm that your new dedicated IP addresses are associated with your account\. 

## Relinquish dedicated IP addresses<a name="dedicated-ip-case-relinquish"></a>

If you no longer need dedicated IP addresses that are associated with your account, you can relinquish them by completing the following steps\.

**Important**  
The process of relinquishing a dedicated IP address can't be reversed\. If you relinquish a dedicated IP address in the middle of a month, we prorate the monthly dedicated IP usage fee, based on the number of days that have elapsed in the current month\.

**To relinquish dedicated IP addresses**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane on the left side of the screen, choose **Configuration**, then choose **Dedicated IPs**\.

1. To open a new case in the Support Center, choose **Request or relinquish dedicated IPs** at the top of the page\.

1. Under **Case classification**, complete the following sections:
   + For **Limit type**, keep **SES Sending Limits**\.
**Note**  
Remaining boxes in this section are optional, and do not apply to relinquishing dedicated IPs\. Leave them blank\.

1. Under **Requests**, complete the following sections:
   + For **Region**, choose the AWS Region that your request applies to\.
**Note**  
Dedicated IP addresses are unique to each AWS Region, so it's important to choose the Region that the dedicated IP address is associated with\.
   + For **Limit**, choose **Desired Maximum Send Rate**\.
   + For **New limit value**, enter any number\. The number that you enter here isn't important—you specify the number of dedicated IPs that you want to relinquish in the next step\.
**Note**  
 A single dedicated IP address can only be used in a single AWS Region\. If you want to relinquish dedicated IP addresses that you used in other AWS Regions, choose **Add another request**\. Then complete the **Region**, **Limit**, and **New limit value** fields for the additional Region\. Repeat this process for each dedicated IP address that you want to relinquish\.

1. Under **Case Description**, for **Use case description**, mention that you want to relinquish existing dedicated IP addresses\. If you currently lease more than one dedicated IP address, include the number of dedicated IP addresses that you want to relinquish\.

1. Under **Contact options**, for **Preferred contact language**, choose whether you want to receive communications for this case in **English** or **Japanese**\.

1. When you finish, choose **Submit**\.

After we receive your request, we send you a message that asks you to confirm that you want to relinquish your dedicated IP addresses\. After you confirm that you want to relinquish the IP addresses, we remove them from your account\.