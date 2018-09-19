# Monitoring Your Amazon SES Sending Limits<a name="monitor-sending-limits"></a>

You can monitor your sending limits by using the Amazon SES console or through the Amazon SES API, whether by calling the Query \(HTTPS\) interface directly or indirectly through an [AWS SDK](https://aws.amazon.com/tools/), the [AWS Command Line Interface](https://aws.amazon.com/cli/), or the [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/)\.

**Important**  
We recommend that you frequently check your sending statistics to ensure that you are not close to your sending limits\. If you are close to your sending limits, see [Increasing Your Amazon SES Sending Limits](increase-sending-limits.md) for information about how to increase them\. Don't wait until you reach your sending limits to consider increasing them\.

## Monitoring Your Sending Limits Using the Amazon SES Console<a name="monitor-sending-limits-console"></a>

The following procedure shows you how to view your sending limits using the Amazon SES console\.

****

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, choose **Sending Statistics**\. Your sending limits are shown under **Your Amazon SES Sending Limits**\.   
![\[AWS Management Console\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/console_dashboard.png)

1. To update the display, choose **Refresh**\.

## Monitoring Your Sending Limits Using the Amazon SES API<a name="monitor-sending-limits-api"></a>

The Amazon SES API provides the `GetSendQuota` action, which returns your sending limits\. When you call `GetSendQuota` action, you receive the following information:
+ Number of emails you have sent during the past 24 hours
+ Sending quota for the current 24\-hour period
+ Maximum send rate

**Note**  
For a description of `GetSendQuota`, see [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\.