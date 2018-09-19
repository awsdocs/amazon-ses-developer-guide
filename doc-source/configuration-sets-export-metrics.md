# Exporting Reputation Metrics for a Configuration Set to CloudWatch<a name="configuration-sets-export-metrics"></a>

Amazon SES automatically exports information about the overall bounce and complaint rates for your entire account to Amazon CloudWatch\. You can use these metrics to create alarms in CloudWatch, or to automatically pause email sending using a Lambda function\.

You can also export reputation metrics for individual configuration sets to CloudWatch\. Exporting reputation data at the configuration set level gives you more control over your sender reputation\.

This section includes procedures for exporting reputation data for individual configuration sets to CloudWatch by using the Amazon SES API\.

## Enabling the Exporting of Reputation Metrics for a Configuration Set<a name="configuration-sets-export-metrics-enabling"></a>

To start exporting reputation metrics for a configuration set, use the `UpdateConfigurationSetReputationMetricsEnabled` API operation\. To access the Amazon SES API, we recommend using the AWS CLI or one of the AWS SDKs\.

This procedure assumes that the AWS CLI is installed on your computer and properly configured\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\.

**To enable the exporting of reputation metrics for a configuration set**
+ At the command line, type the following command: aws ses update\-configuration\-set\-reputation\-metrics\-enabled \-\-configuration\-set\-name *ConfigSet* \-\-enabled

  Replace *ConfigSet* in the preceding command with the name of the configuration set for which you want to start exporting reputation metrics\.

## Disabling the Exporting of Reputation Metrics for a Configuration Set<a name="configuration-sets-export-metrics-disabling"></a>

You can also use the `UpdateConfigurationSetReputationMetricsEnabled` API operation to disable the exporting of reputation metrics for a configuration set\.

**To disable the exporting of reputation metrics for a configuration set**
+ At the command line, type the following command: aws ses update\-configuration\-set\-reputation\-metrics\-enabled \-\-configuration\-set\-name *ConfigSet* \-\-no\-enabled

  Replace *ConfigSet* in the preceding command with the name of the configuration set for which you want to disable the exporting of reputation metrics\.