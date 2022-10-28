# Monitoring your sending statistics using the Amazon SES console<a name="monitor-sending-activity-console"></a>

From the Amazon SES console's **Account dashboard** page and **Reputation metrics** page, you can monitor all your email sending, usage, statistics, SMTP settings, overall account health, and reputation metrics\. The following sections describe the metrics and statistics provided on each of these console pages\.

It should be noted that while both the [Account dashboard](#account-dashboard) and [Reputation metrics](#reputation-metrics) console pages contain bounce and complaint metrics, there is a subtle difference between these two sets of bounce and complaint rates as explained below:
+ **Account dashboard page** – based on the date range selected, you can view what the bounce and complaint rates were in the past showing the metric progression of change leading up to the present time\.
+ **Reputation metrics page** – bounce and complaint rates based on the latest data point received from calculating your overall historic average at a high level \(this shouldn’t be confused with your regular bounce/complaint rate, which corresponds to precise bounce/complaint events as they occur in real\-time as shown on the **Account dashboard** page\)\.

As a simple example to compare either the bounce or complaint rates between the **Reputation metrics** page and the **Account dashboard** page, let’s say the rate was 2% yesterday and is 1% now, on the **Reputation metrics** page, you'll only see the current rate of 1%, but on the **Account dashboard** page, the graphs will plot the charted progression showing a rate of 2% for yesterday and 1% for today\.

## Account dashboard<a name="account-dashboard"></a>

You can monitor the number of emails sent from your account, as well as the percentage of your sending quota that's been used, directly from the SES console's **Account dashboard** page in the *Daily email usage* pane\. Delivery and rejection rates for your account can be monitored in the *Sending Statistics* pane, as well as other key factors related to your email sending in the following panes:
+ **Sending limits** – contains the following quotas applicable to sending mail through SES:
  + *Daily sending quota* \- maximum number of emails that you can send in a 24\-hour period\.
  + *Maximum send rate* \- maximum number of emails that can be send from your account each second\.
+ **Account health** – the status of your SES account:
  + `Healthy` \- there are no reputation\-related issues that currently impact your account\.
  + `Under review` \- potential issues have been identified with your SES account \- your account is under review while you work on correcting the issues\.
  + `Paused` \- your account's ability to send email is currently paused because of an issue with the email sent from your account\. When the issue's been corrected, you can request that your account's ability to send email is resumed\.
+ **Daily email usage** – to check your daily usage to ensure you aren’t approaching your sending limits:
  + *Emails sent* \- total number of emails sent in a 24\-hour period\.
  + *Remaining sends* \- total number of remaining emails available to be sent in a 24\-hour period\.
  + *Sending quota used* \- percentage of your daily sending quota used\.
+ **SMTP settings** – if you want to use an SMTP\-enabled programming language, email server, or application to connect to the Amazon SES SMTP interface, the following information is provided:
  + SMTP endpoint
  + STARTTLS Port
  + Transport Layer Security \(TLS\)
  + TLS Wrapper Port
  + Authentication links provided for SMTP and IAM credentials
+ **Sending statistics** – comprised of graphs that show the progression of four essential metrics in a time\-ordered set of data points representing the values of a monitored event type producing statistics for the selected date range using an aggregation period of *1 hour*\. You can select a data range with start values from `Last 1 day` to `Last 14 days` to filter the charts below:
  + *Sends* \- sum of successful email send requests for the date range selected\.
  + *Rejects* \- average rate of rejected send requests by SES based on `Rejects/Sends * 100` for the date range selected\.
  + *Bounces* \- average rate derived from your overall historic sender reputation metrics showing the progression for the date range selected\.
  + *Complaints* \- average rate derived from your overall historic sender reputation metrics showing the progression for the date range selected\.

Each of these charts contain a **View in CloudWatch** button that will open the respective metric in the Amazon CloudWatch console allowing detailed data to be viewed, customized metric math performed, and [the creation of alarms in CloudWatch](reputationdashboard-cloudwatch-alarm.md)\.

## Reputation metrics<a name="reputation-metrics"></a>

In addition to bounce and complaint rates, the **Reputation metrics** page also provides other high\-level visibility into key factors affecting your reputation consisting of the following panes:
+ **Summary** – provides an overview of your reputation health\.
  + *Status* \- overall reputation health based on historic bounce and complaint rates:
    + `Healthy` \- both metrics are within normal levels\.
    + `Under review` \- one or both metrics have automatically caused your account to be placed under review\.
    + `At risk` \- one or both metrics have reached unhealthy levels and your account’s ability to send email may be at risk\.
  + *Messages sent* — shows the representative volume of email based on your typical sending practices to calculate your historic bounce and complaint rates\.
  + *Sent over period* — shows the period of time over which your representative volume of mail was sent\. To be fair to both high\- and low\-volume senders, this sending period is different for each account and may change as the account's sending patterns change\.
+ **Account\-level tab contents:**
  + Bounce rate
    + *Status* \- indicates the health of your bounce rate using the same values as described for the Summary pane\.
    + *Historic bounce rate* \- percentage of emails from your account that resulted in a hard bounce calculated from your overall historic average based on a representative volume that represents your typical sending practices\.
  + Complaint rate
    + *Status* \- Indicates the health of your complaint rate using the same values as described for the Summary pane\.
    + *Historic bounce rate* \- percentage of emails sent from your account that resulted in recipients reporting them as spam calculated from your overall historic average based on a representative volume that represents your typical sending practices\.
+ **Configuration set tab contents:**
  + Reputation by configuration set
    + *Configuration set* \- lets you type or select a configuration set that have reputation metrics enabled so you can see summary, bounce, and complaint data based on the emails sent using the selected configuration set\. The resulting panes that appear after selecting a configuration set are the same as described above for the Reputation metrics page except they are based only on email sent with the selected configuration set as apposed to your overall account\-level sending metrics\.

## Using the console to monitor send and reputation metrics<a name="console-stats-metrics"></a>

The following procedures will get you started in exploring your send and reputation metrics either using the **Account dashboard** page for metrics based on recent history \(up to 14 days\), or use the **Reputation metrics** page for metrics based on your overall history to the present time\.

**To view emails sent and sending quota used**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, choose **Account dashboard**\. Your usage statistics are shown in the **Daily email usage** section\.

**To view count of sends, rates of rejects, bounces, and complaints**

1. In the navigation pane, choose **Account dashboard**\.

1. In the **Sending statistics** section, use the **Date range** dropdown to select a start value for a date range to filter the four charts directly below the **Sending statistics** section\.

1. Based on the date range selected, you can view what the counts and rates were in the past showing the metric progression of change leading up to the present time\.

1. In any of the charts, choose the **View in CloudWatch** button to open the respective metric in the **Amazon CloudWatch** console where you can view detailed data, perform customized metric math, and [create monitoring alarms in CloudWatch](reputationdashboard-cloudwatch-alarm.md)\.

**To view overall historic bounce and complaint rates**

1. In the navigation pane, choose **Reputation metrics**\.

1. In the **Bounce rate** pane you can view the percentage of emails sent from your account that resulted in a hard bounce, and in the **Complaint rate** pane you can view the percentage of emails sent from your account that resulted in recipients reporting them as spam; both metrics calculated from a representative volume of email based on your typical sending practices\.

1. In either of the panes, choose the **View in CloudWatch** button to open the respective metric in the **Amazon CloudWatch** console where you can view detailed data, perform customized metric math, and [create monitoring alarms in CloudWatch](reputationdashboard-cloudwatch-alarm.md)\.

**To view reputation metrics by configuration sets**

1. In the navigation pane, choose **Reputation metrics**\.

1. On the Reputation metrics page, select the **Configuration set** tab\.

1. In the **Reputation by configuration set** pane, click inside the **Configuration set** field and either start typing for, or select, a configuration set that has reputation metrics enabled\.

1. After selecting the configuration set, it will load the Summary, Bounce, and Complaint panes showing metrics based only on email sent with the selected configuration set\.