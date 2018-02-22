# Requesting and Relinquishing Dedicated IP Addresses<a name="dedicated-ip-case"></a>

This section describes how to request and relinquish dedicated IP addresses by submitting a request in the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\. Note that you're charged an additional monthly fee for each dedicated IP address that you lease for use with Amazon SES\. For more information about the costs associated with dedicated IP addresses, see [Amazon SES Pricing](https://aws.amazon.com/ses/pricing/#Optional_Services)\.

## Best Practices for Working with Dedicated IP Addresses<a name="dedicated-ip-case-best-practices"></a>

Although there's no minimum commitment, we recommend that you lease three or more dedicated IP addresses in order to maximize the delivery of your email\.

Each AWS Region that Amazon SES is available in contains at least three *Availability Zones* \(*zones*\)\. When you lease more than one dedicated IP address, we distribute them evenly across the Availability Zones for your region\. 

If you create a [dedicated IP pool](dedicated-ip-pools.md) that contains only one address, and the zone where that address is based is temporarily unavailable, emails that you attempt to send using that pool might not be sent\. However, if you create a pool that contains three addresses, you can ensure that each address is in a separate zone\. In this case, even in the unlikely event that two of the three zones are unavailable, you'll still be able to send email from the remaining address\. 

Service interruptions are rare, but it's a good practice to use redundant systems, especially if you send time\-sensitive or mission\-critical email\. For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)\.

## Requesting Dedicated IP Addresses<a name="dedicated-ip-case-request"></a>

The following steps show how to request dedicated IP addresses by submitting an [SES Sending Limits Increase case](https://aws.amazon.com/ses/extendedaccessrequest/)\. You can use this process to request as many dedicated IP addresses as you need\.

**To request dedicated IP addresses**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Open an [SES Sending Limits Increase case](https://aws.amazon.com/ses/extendedaccessrequest/) in Support Center\.

1. In the case submission form, make the following selections:

   + For **Regarding**, choose **Service Limit Increase**\.

   + For **Limit Type**, choose **SES Sending Limits**\.

   + For **Region**, choose the AWS Region in which you use Amazon SES\.
**Note**  
Dedicated IP addresses are unique to each AWS Region, so it is important to select the region in which you use Amazon SES\.  
For example, if you have a dedicated IP address for use in the US West \(Oregon\) region, you will not be able to use that address to send email from the US East \(N\. Virginia\) region\.  
If you need to request dedicated IP addresses for use in more than one region, submit a separate request form for each region\.

   + For **Limit**, choose **Desired Daily Sending Quota**\.

   + For **New limit value**, enter the approximate number of emails you plan to send per day from the dedicated IP addresses that you are requesting\.

   + For **Mail Type**, choose the type of email you plan to send using your dedicated IP address\. If multiple values apply, choose the type that will make up the majority of your email sending\.

   + For **Website URL**, type the URL of your website\. Providing this information will help us better understand the type of content you plan to send\.

   + For **My email sending complies with the AWS Service Terms and AUP**, choose the option that applies to your use case\.

   + For **I only send to recipients who have specifically requested my mail**, choose the option that applies to your use case\.

   + For **I have a process to handle bounces and complaints**, choose the option that applies to your use case\.

   + For **Use Case Description**, state that you want to request dedicated IP addresses, and indicate the number of dedicated IP addresses that you are requesting\. Next, describe the ways in which you will use dedicated IP addresses to send email using Amazon SES\. Include information about why you want to use dedicated IP addresses as opposed to shared IP addresses; this information will help us better understand your use case\.

   + For **Support Language**, choose your preferred language\.

   + For **Contact method**, choose **Web**\.

   When you finish, choose **Submit**\.

Once you submit the form, we will evaluate your request\. We will reply to your request in Support Center\. If your request is granted, the reply will confirm that your dedicated IP addresses are now associated with your account\. 

## Relinquish Dedicated IP Addresses<a name="dedicated-ip-case-relinquish"></a>

If you no longer need dedicated IP addresses that are assigned to your account, you can relinquish them by completing the following steps\.

**To relinquish dedicated IP addresses**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Open an [SES Sending Limits Increase case](https://aws.amazon.com/ses/extendedaccessrequest/) in Support Center\.

1. In the case submission form, make the following selections:

   + For **Regarding**, choose **Service Limit Increase**\.

   + For **Limit Type**, choose **SES Sending Limits**\.

   + For **Region**, choose the AWS Region in which you use Amazon SES\.
**Note**  
Dedicated IP addresses are unique to each AWS Region, so it is important to select the region in which you use Amazon SES\.  
If you need to relinquish dedicated IP addresses in more than one region, submit a separate request form for each region\.

   + For **Limit**, choose **Desired Daily Sending Quota**\.

   + For **New limit value**, enter any number\. The number you enter here is not important—you will specify how many dedicated IPs you want to relinquish in the Use Case Description field\.

   + For **Use Case Description**, state that you want to relinquish dedicated IP addresses, and then state the number of dedicated IP addresses you want to relinquish\.

   + For **Support Language**, choose your preferred language\.

   + For **Contact method**, choose **Web**\.

   When you finish, choose **Submit**\.

   After we evaluate your request, you will receive a reply within the case asking you to confirm that you want to release the number of dedicated IP addresses that you specified\. You will receive a case reply confirming that your dedicated IP addresses have been released\.