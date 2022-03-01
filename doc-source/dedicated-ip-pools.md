# Creating Dedicated IP Pools<a name="dedicated-ip-pools"></a>

If you purchased several dedicated IP addresses to use with Amazon SES, you can create groups of those addresses\. These groups are called *dedicated IP pools*\. A common scenario is to create one pool of dedicated IP addresses for sending marketing communications, and another for sending transactional emails\. Your sender reputation for transactional emails is then isolated from that of your marketing emails\. In this scenario, if a marketing campaign generates a large number of complaints, the delivery of your transactional emails is not impacted\. 

This section contains procedures for creating dedicated IP pools\.

**Note**  
You can also create configuration sets that use a pool of IP addresses that are shared by all Amazon SES customers\. The shared IP pool is useful in situations where you need to send email that doesn't align with your usual sending behaviors\. For information about using the shared IP pool with a configuration set, see [Managing dedicated IP pools](managing-ip-pools.md)\.

**To create a dedicated IP pool using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane on the left side of the screen, under **Email Sending**, choose **Dedicated IPs**\.

1. Choose **Create a New IP Pool**\.

1. On the **IP Pool Name** page, for **Pool name**, type a descriptive name for the dedicated IP pool, and then choose **Next**\.

1. On the **Add Dedicated IPs** page, check the box next to each IP address you want to add to the pool, and then choose **Next**\.
**Note**  
Dedicated IP addresses that you haven't yet assigned to a pool are included in the **ses\-default\-dedicated\-pool**\. If you send an email using a configuration set that doesn't specify a sending pool, or if you send an email without specifying a configuration set at all, Amazon SES sends the email from one of the addresses in the **ses\-default\-dedicated\-pool**\.  
A dedicated IP address can only belong to one pool\. If you select a dedicated IP address that's associated with a different pool, Amazon SES overwrites that setting, and associates the address with the pool that you're creating\.

1. On the **Assign to a configuration set** page, do one of the following:
   + Select **Add this pool to an existing configuration set** to associate the dedicated IP pool with an existing configuration set\. Then, under **Existing configuration sets**, choose the configuration set that you want to associate the IP pool with\.
   + Select **Create a new configuration set** to create a configuration set and associate the dedicated IP pool with it\. For **Configuration set name**, type a descriptive name for the configuration set\.

   When you finish, choose **Next**\.

1. On the **Review** page, verify the settings of the dedicated IP pool\. When you are ready to create the IP pool, choose **Create**\.
