# Managing dedicated IP pools<a name="managing-ip-pools"></a>

You can use IP pools to create groups of dedicated IP addresses for sending specific types of email\. You can also use a pool of IP addresses that are shared by all Amazon SES customers\.

## Assigning an IP pool to an existing configuration set<a name="assign-ip-pools"></a>

You can use the Amazon SES console to associate an IP pool with an existing configuration set\. 

**To assign an IP pool to a configuration set**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. In the list of configuration sets, choose the configuration set that you want to associate with an IP pool\.

1. On the **Sending IP pool** tab, for **Pool name**, choose from one of the following options:
   + *A specific dedicated IP pool* – When you select an existing dedicated IP pool, emails that use the configuration set are sent using only the dedicated IP addresses that belong to that pool\. For procedures for creating new IP pools, see [Creating Dedicated IP Pools](dedicated-ip-pools.md)\.
   + **ses\-default\-dedicated\-pool** – This pool contains all of the dedicated IP addresses for your account that do not already belong to an IP pool\. If you send an email using a configuration set that is not associated with a pool, or if you send an email without specifying a configuration set at all, the email is sent from one of the addresses in the default pool\.
   + **ses\-shared\-pool** – This pool contains a large set of IP addresses that are shared among all Amazon SES customers\. This option may be useful when you need to send email that doesn't align with your usual sending behaviors\.

   When you are finished, choose **Assign**\.

## Modifying IP pool assignments<a name="edit-config-set-ip-pools"></a>

You can also use the Amazon SES console to assign a different pool to a configuration set that is already associated with a pool\. Assigning a different pool to a configuration set overwrites the previous association\.

**To edit an IP pool assignment**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. In the list of configuration sets, choose the configuration set that you want to modify\.

1. On the **Sending IP pool** tab, under **Assign an IP pool**, choose the **edit** icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/config_set_edit_icon.png)\)\.

1. For **Pool name**, select the pool that you want to use, and then choose **Assign**\.
