# Managing configuration sets in Amazon SES<a name="managing-configuration-sets"></a>

This section contains procedures for creating configuration sets, viewing a list of your existing configuration sets, viewing the details of individual configuration sets, and deleting configuration sets\.

## Creating a configuration set<a name="event-publishing-managing-configuration-sets-creating"></a>

You can use the Amazon SES console or the `CreateConfigurationSet` API to create new configuration sets\.

**To create a configuration set by using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, choose **Configuration Sets**\.

1. Choose **Create Configuration Set**\.

1. For **Configuration set name**, type a name for the configuration set\.
**Note**  
The name can contain up to 64 alphanumeric characters\. It can also contain hyphens \(\-\) and underscores \(\_\)\. Names can't contain spaces, accented characters, or any other special characters\.

You can also use the `CreateConfigurationSet` API to create configuration sets\. A common way to call this API is by using the AWS CLI\.

**To create a configuration set by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses create-configuration-set --configuration-set Name=ConfigSet
  ```

  In the preceding command, replace *ConfigSet* with the name that you want to give the configuration set\.
**Note**  
The name can contain up to 64 alphanumeric characters\. It can also contain hyphens \(\-\) and underscores \(\_\)\. Names can't contain spaces, accented characters, or any other special characters\.

For more information about using the `CreateConfigurationSet` API to create configuration sets, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateConfigurationSet.html)\.

## Viewing a list of your configuration sets<a name="event-publishing-managing-configuration-sets-viewing"></a>

You can use the Amazon SES console or you can use the `ListConfigurationSets` API to view a list of your configuration sets\.

**To view your configuration sets using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

   In the details pane, you will see a list of your configuration sets\.

You can also use the `ListConfigurationSets` API to view a list of configuration sets\. A common way to call this API is by using the AWS CLI\.

**To view a list of configuration sets by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses list-configuration-sets
  ```

For more information about using `ListConfigurationSets` API to list your configuration sets, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListConfigurationSets.html)\.

## Viewing the details of a configuration set<a name="event-publishing-managing-configuration-sets-describing"></a>

You can use the Amazon SES console to view the details of a configuration set, or you can use the `DescribeConfigurationSet` API to describe a configuration set\. 

**Viewing the details of a configuration set using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. In the details pane, choose the expand icon next to the configuration set\.

   You will see the details of the configuration set\.

You can also use the `DescribeConfigurationSet` API to show more information about a configuration set\. A common way to call this API is by using the AWS CLI\.

**To obtain more information about a configuration set by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses describe-configuration-set --configuration-set-name ConfigSet
  ```

  In the preceding command, replace *ConfigSet* with the name of the configuration set that you want to learn more about\.

For information about how to use the `DescribeConfigurationSet` API to describe a configuration set, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_DescribeConfigurationSet.html)\.

## Deleting a configuration set<a name="event-publishing-managing-configuration-sets-deleting"></a>

You can use the Amazon SES console or the `DeleteConfigurationSet` API to delete a configuration set\. 

**To delete a configuration set using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. In the details pane, choose the configuration set\.

1. From the **Actions** menu, choose **Delete**, and then confirm that you want to delete the configuration set\.

You can also use the `DeleteConfigurationSet` API to delete configuration sets\. A common way to call this API is by using the AWS CLI\.

**To delete a configuration set by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses delete-configuration-set --configuration-set ConfigSet
  ```

  In the preceding command, replace *ConfigSet* with the name of the configuration set that you want to delete\.

For more information using the `DeleteConfigurationSet` API to delete a configuration set, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteConfigurationSet.html)\.
