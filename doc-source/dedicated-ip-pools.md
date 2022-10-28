# Creating standard dedicated IP pools for dedicated IP addresses \(standard\)<a name="dedicated-ip-pools"></a>

If you purchased several dedicated IP addresses \(standard\) to use with Amazon SES, you can create groups of those addresses, called *dedicated IP pools*\.   Grouping dedicated IPs \(standard\) together in a pool makes them easier to manage\. A common scenario is to create one pool for sending marketing communications, and another for sending transactional emails\. Your sender reputation for transactional emails is then isolated from that of your marketing emails\. In this scenario, if a marketing campaign generates a large number of complaints, the delivery of your transactional emails is not impacted\.

This section contains procedures for creating dedicated IP pools\.

**Note**  
You can also create configuration sets that use a pool of IP addresses that are shared by all SES customers\. The shared IP pool is useful for situations in which you need to send email that doesn't align with your usual sending behaviors\. For information about using the shared IP pool with a configuration set, see [Assigning IP pools in Amazon SES](managing-ip-pools.md)\.

**To create a dedicated IP pool for dedicated IPs \(standard\) using the SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Dedicated IPs**\.

1. Select the **Standard IP pools** tab on the **Dedicated IPs** page\.

1. In the **All Standard dedicated IP pools** panel, choose **Create Standard IP pool**\.

1. In the **Pool details** panel,

   1. Choose **Standard \(self managed\)** in the **Scaling mode** field\.

   1. Enter a name for your IP pool in the **IP pool name** field\.
**Note**  
The IP pool name must be unique and can't be a duplicate of a managed IP pool name in your account\.

   1. \(Optional\) If you have existing standard dedicated IP addresses that you want to add to this IP pool, select them from the dropdown list in the **Dedicated IP addresses** field\.
**Note**  
If you select an IP address that's already associated with an IP pool, it will now only be associated with this IP pool\.

1. \(Optional\) You can associate this IP pool with a configuration set by selecting one from the dropdown list in the **Configuration sets** field\.
**Note**  
If you select a configuration set that's already associated with an IP pool, it will now only be associated with this IP pool\.
To add or remove associated configuration sets after this IP pool is created, edit the configuration set's [**Sending IP pool**](managing-configuration-sets.md#console-detail-configuration-sets) parameter\.
If you haven’t created any configuration sets yet, see [Using configuration sets in Amazon SES](using-configuration-sets.md)\.

1. \(Optional\) You can add one or more **Tags** to this IP pool by including a tag key and an optional value for the key\.

   1. Choose **Add new tag** and enter the **Key**\. You can also add an optional **Value** for the tag\.

   1. To add the tag, choose **Save changes**\.

      You can add up to 50 tags\. You can remove a tag by choosing **Remove**\.

1. Select **Create pool**\.
**Note**  
After you create a standard IP pool, it can't be converted to a managed IP pool\.