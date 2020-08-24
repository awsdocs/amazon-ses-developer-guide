# Monitoring your Amazon SES sending quotas<a name="manage-sending-quotas-monitor"></a>

You can monitor your sending quotas by using the Amazon SES console or through the Amazon SES API, whether by calling the Query \(HTTPS\) interface directly or indirectly through an [AWS SDK](https://aws.amazon.com/tools/), the [AWS Command Line Interface](https://aws.amazon.com/cli/), or the [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/)\.

**Important**  
We recommend that you frequently check your sending statistics to ensure that you are not close to your sending quotas\. If you are close to your sending quotas, see [Increasing your Amazon SES sending quotas](manage-sending-quotas-request-increase.md) for information about how to increase them\. Don't wait until you reach your sending quotas to consider increasing them\.

## Monitoring your sending quotas using the Amazon SES console<a name="manage-sending-quotas-monitor-console"></a>

The following procedure shows you how to view your sending quotas using the Amazon SES console\.

****

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, choose **Sending Statistics**\. Your sending quotas are shown under **Your Amazon SES Sending Limits**\.   
![\[AWS Management Console\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/console_dashboard.png)

1. To update the display, choose **Refresh**\.

## Monitoring your sending quotas using the Amazon SES API<a name="manage-sending-quotas-monitor-api"></a>

The Amazon SES API provides the `GetSendQuota` action, which returns your sending quotas\. When you call `GetSendQuota` action, you receive the following information:
+ Number of emails you have sent during the past 24 hours
+ Sending quota for the current 24\-hour period
+ Maximum send rate

**Note**  
For a description of `GetSendQuota`, see [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\.