# Dedicated IP addresses \(managed\) for Amazon SES<a name="managed-dedicated-sending"></a>

*Dedicated IP addresses \(managed\)* is an Amazon SES feature that automatically sets up and manages dedicated IP addresses on your behalf to provide a quick and easy way to start using dedicated IP addresses that are managed by SES\. This helps to ensure that your dedicated IP addresses are used efficiently and optimally for how you send email\.  

Dedicated IPs \(managed\) and *dedicated IPs \(standard\)* both refer to dedicated IP addresses that you lease in SES for [an additional cost](https://aws.amazon.com/ses/pricing), but differ in how they're implemented and managed\. While there are shared benefits common to both, they each have unique advantages to offer depending on your type of email sending, as discussed in [Dedicated IP addresses for Amazon SES](dedicated-ip.md)\.

## Benefits and features of dedicated IPs \(managed\)<a name="dedicated-ip-benefits-mds"></a>

The dedicated IP addresses that you create with dedicated IPs \(managed\) automate management tasks to help ensure that your dedicated IP addresses are used in a way that's optimal for how you send email:
+ **Easy onboarding** – To get started with dedicated IPs \(managed\), you create a managed IP pool directly from the SES console\. Dedicated IP addresses are automatically allocated to the pool\. You can start sending with the managed IP pool without having to open a request case through the AWS Support Center\.
+ **Auto\-scaling per ISP** – You don't have to manually monitor or scale your dedicated IP pools\. The managed IP pool scales out and in automatically based on usage\. It also takes into consideration ISP\-specific policies\. For example, if SES detects that an ISP supports a low daily send quota, the pool scales out to better distribute traffic to that ISP across more IP addresses\.
+ **Intelligent warmup** – Dedicated IPs \(managed\) start to send mail to ISPs based on their capacity\. That is, how much they are currently warmed up\. They automatically keep track of the level of warmup for each ISP individually\. Additionally, the dedicated IPs \(managed\) feature provides information about your reputation at an effective daily rate with top ISPs in the form of Amazon CloudWatch metrics and built\-in dashboards\. 
  + **Warmup per ISP** – SES tracks the reputation for each IP in the managed IP pool for each ISP individually\. For example, if you've been sending all your traffic to Gmail, the IP addresses are considered warmed up only for Gmail and cold for other ISPs\. If you change your traffic pattern by ramping up email sent to Hotmail, SES ramps up traffic slowly for Hotmail, as the IP addresses are not warmed up yet\.
  + **Adaptive warmup** – The warmup adjustment is adaptive and takes into account actual sending patterns\. When sending volume to an ISP drops, the warmup percentage also drops for that ISP\. In the early phase of warmup, any sending that's excessive based on the current level of warmup is sent through the IP addresses that are shared with other Amazon SES users\. In later stages of warmup, any sending that's excessive is proactively slowed down and retried later\.
+ **Automatic request & relinquish of dedicated IP addresses** – You don't need to request or relinquish managed dedicated IP addresses through the AWS Support Center, as is required when using dedicated IPs \(standard\)\. When onboarding with dedicated IPs \(managed\) directly from the SES console, CLI, or API, you are automatically allocated dedicated IP addresses and charged a fee based on the volume of messages that you send\. When you delete an IP pool that's created by dedicated IPs \(managed\) or opt out of dedicated IPs \(managed\), your allocated IP addresses are automatically relinquished and charges cease immediately\.

## Creating a managed IP pool to enable dedicated IPs \(managed\)<a name="dedicated-ip-pools-mds"></a>

To enable dedicated IPs \(managed\), you create a managed IP pool and specify *Managed* for its scaling mode\. After you create a managed pool, the feature determines how many IPs you require based on your sending patterns and dynamically scales to your requirements\. To use your managed pool to send email, you must associate the managed pool with a configuration set, and then specify that configuration set when sending email\. The configuration set can also be applied to a sending identity by using a [default configuration set](managing-configuration-sets.md#default-config-sets)\.

**To create a managed IP pool using the SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Dedicated IPs**\.

1. On the **Dedicated IPs** page, choose **Managed IP pools**\.

1. Choose **Create managed IP pool**\.

1. In the **Pool details** panel,

   1. Choose **Managed \(auto managed\)** in the **Scaling mode** field\.

   1. Enter a name for your managed pool in the **IP pool name** field\.
**Note**  
The IP pool name must be unique\. It can't be a duplicate of a standard dedicated IP pool name in your account\.
You can't have more than 5 managed IP pools per AWS Region in your account\.

1. \(Optional\) You can associate this managed IP pool with a configuration set by choosing one from the dropdown list in the **Configuration sets** field\.
**Note**  
If you choose a configuration set that's already associated with an IP pool, it will become associated with this managed pool, and no longer be associated with the previous pool\.
To add or remove associated configuration sets after this managed pool is created, edit the configuration set's [**Sending IP pool**](managing-configuration-sets.md#console-detail-configuration-sets) parameter in the **General details** panel\.
If you haven’t created any configuration sets yet, see [Using configuration sets in Amazon SES](using-configuration-sets.md)\.

1. \(Optional\) You can add one or more **Tags** to your IP pool by including a tag key and an optional value for the key\.

   1. Choose **Add new tag** and enter the **Key**\. You can also add an optional **Value** for the tag\.

   1. To add the tag, choose **Save changes**\.

      You can add up to 50 tags\. You can remove a tag by choosing **Remove**\.

1. Select **Create pool**\.
**Note**  
After you create a managed IP pool, it can't be converted to a standard IP pool\.
When using dedicated IPs \(managed\), you can't have more than 10,000 sending identities \(domains and email addresses, in any combination\) per AWS Region in your account\.

After you create your managed IP pool, you can track the sending performance of your managed dedicated IP addresses by [monitoring email sending using event publishing](monitor-using-event-publishing.md)\.