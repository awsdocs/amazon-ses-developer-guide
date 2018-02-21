# Managing Amazon SES Configuration Sets<a name="managing-configuration-sets"></a>

This section contains procedures for viewing a list of your existing configuration sets, viewing the details of those configuration sets, and deleting configuration sets\.

## Viewing a List of Your Configuration Sets<a name="event-publishing-managing-configuration-sets-viewing"></a>

You can use the Amazon SES console or you can use the `ListConfigurationSets` API to view a list of your configuration sets\.

**To view your configuration sets using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

   In the details pane, you will see a list of your configuration sets\.

For information about how to use the `ListConfigurationSets` API to list your configuration sets, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/API_ListConfigurationSets.html)\.

## Viewing the Details of a Configuration Set<a name="event-publishing-managing-configuration-sets-describing"></a>

You can use the Amazon SES console to view the details of a configuration set, or you can use the `DescribeConfigurationSet` API to describe a configuration set\. 

**Viewing the details of a configuration set using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. In the details pane, choose the expand icon next to the configuration set\.

   You will see the details of the configuration set\.

For information about how to use the `DescribeConfigurationSet` API to describe a configuration set, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/API_DescribeConfigurationSet.html)\.

## Deleting a Configuration Set<a name="event-publishing-managing-configuration-sets-deleting"></a>

You can use the Amazon SES console or the `DeleteConfigurationSet` API to delete a configuration set\. 

**To delete a configuration set using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. In the details pane, choose the configuration set\.

1. From the **Actions** menu, choose **Delete**, and then confirm that you want to delete the configuration set\.

For information about how to use the `DeleteConfigurationSet` API to delete a configuration set, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteConfigurationSet.html)\.