# Warming up dedicated IP addresses<a name="dedicated-ip-warming"></a>

When determining whether to accept or reject a message, email service providers consider the reputation of the IP address that sent it\. One of the factors that contributes to the reputation of an IP address is whether the address has a history of sending high\-quality email\. Email providers are less likely to accept mail from new IP addresses that have little or no history\. Email sent from IP addresses with little or no history might end up in recipients' junk mail folders, or might be blocked altogether\.

When you start sending email from a new IP address, you should gradually increase the amount of email you send from that address before using it to its full capacity\. This process is called *warming up* the IP address\.

The amount of time required to warm up an IP address varies between email providers\. For some email providers, you can establish a positive reputation in around two weeks, while for others it may take up to six weeks\. When warming up a new IP address, you should send emails to your most active users to ensure that your complaint rate remains low\. You should also carefully examine your bounce messages and send less email if you receive a high number of blocking or throttling notifications\. For information about monitoring your bounces, see [Monitoring your Amazon SES sending activity](monitor-sending-activity.md)\.

## Automatically warm up dedicated IP addresses<a name="dedicated-ip-auto-warm-up"></a>

When you request dedicated IP addresses, Amazon SES automatically warms them up to improve the delivery of emails you send\. The automatic IP address warm\-up feature is enabled by default\. 

The steps that happen during the automatic warm\-up process depend on whether you already have dedicated IP addresses:
+ When you request dedicated IP addresses for the first time, Amazon SES distributes your email sending between your dedicated IP addresses and a set of addresses that are shared with other Amazon SES customers\. Amazon SES gradually increases the number of messages sent from your dedicated IP addresses over time\.
+ If you already have dedicated IP addresses, Amazon SES distributes your email sending between your existing dedicated IPs \(which are already warmed up\) and your new dedicated IPs \(which are not warmed up\)\. Amazon SES gradually increases the number of messages sent from your new dedicated IP addresses over time\.

**Note**  
Automatic IP warm\-up is a time\-based process\. The warm\-up percentage steadily increases over 45 days independently from your sending volume\.

After you warm up a dedicated IP address, you should send around 1,000 emails every day to each email provider that you want to maintain a positive reputation with\. You should perform this task on each dedicated IP address that you use with Amazon SES\. 

You should avoid sending large volumes of email immediately after the warm\-up process is complete\. Instead, slowly increase the number of emails you send until you reach your target volume\. If an email provider sees a large, sudden increase in the number of emails being sent from an IP address, they may block or throttle the delivery of messages from that address\.

## Disable the automatic warm\-up process<a name="dedicated-ip-enable"></a>

When you purchase new dedicated IP addresses, Amazon SES automatically warms them up for you\. If you prefer to warm up dedicated IP addresses yourself, you can disable the automatic warm\-up feature\.

**Important**  
If you disable the automatic warm up feature, you're responsible for warming up your dedicated IP addresses yourself\. If you send email from addresses that haven't been warmed up, you may experience poor delivery rates\.

**To disable the automatic warm\-up feature**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation bar on the left, choose **Dedicated IPs**\.

1. Choose **Disable auto warm\-up**\.

## Restart the automatic warm\-up process<a name="dedicated-ip-restart-auto-warm-up"></a>

You can restart the automatic IP warm\-up process for a set of IP addresses that belong to a dedicated IP pool\.

**To restart the automatic warm\-up process**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation bar on the left, choose **Dedicated IPs**\.

1. Under **All dedicated IPs**, select the dedicated IP for which you want to restart the warm\-up process, and then choose **Edit warm up**\. Enter a number under **Warm\-up percentage** to specify your desired sending volume for warm\-up\. 

1. Choose **Save changes**\.The status of the automatic warm\-up process is in the **Warm Up status** column; when the warm\-up process is finished, this column will say Complete\.