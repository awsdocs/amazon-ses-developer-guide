# Creating dedicated IP pools<a name="dedicated-ip-pools"></a>

If you purchased several dedicated IP addresses to use with Amazon SES, you can create groups of those addresses\. These groups are called *dedicated IP pools*\. A common scenario is to create one pool of dedicated IP addresses for sending marketing communications, and another for sending transactional emails\. Your sender reputation for transactional emails is then isolated from that of your marketing emails\. In this scenario, if a marketing campaign generates a large number of complaints, the delivery of your transactional emails is not impacted\. 

This section contains procedures for creating dedicated IP pools\.

**Note**  
You can also create configuration sets that use a pool of IP addresses that are shared by all Amazon SES customers\. The shared IP pool is useful in situations where you need to send email that doesn't align with your usual sending behaviors\. For information about using the shared IP pool with a configuration set, see [Manage IP pools in Amazon SES](managing-ip-pools.md)\.

**To create a dedicated IP pool using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane on the left side of the screen, choose **Configuration**, then choose **Dedicated IPs**\.

1. Choose the **IP Pools** tab\.

1. Select **Create IP Pool**\.

1. Under **Pool details**, enter the **IP pool name** you want\. The name must be a unique 64\-character name of only lowercase letters, numbers, periods, underscores, and dashes\.

1. For **Dedicated IPs**, choose the IPs to add to the pool\.
**Note**  
 If you select an IP address that’s already associated with a pool, SES overwrites that setting and associates the address with the new IP pool\.

1. For **Associated configuration sets**, choose the configuration set that you want to associate with the IP pool\. You can assign an IP pool to one or more configuration sets, so emails that use those sets are sent using only the IP addresses that belong to the assigned pool\.

1. \(Optional\) You can add one or more **Tags** to your IP pool by including a tag key and an optional value for the key\.

   1. Choose **Add new tag** and enter the **Key**\. You can also add an optional **Value** for the tag\.

   1. To add the tag, choose **Save changes**\.

      You can add up to 50 tags\. You can remove a tag by choosing **Remove**\.

1. When you're ready to create the IP pool, choose **Create pool**\.