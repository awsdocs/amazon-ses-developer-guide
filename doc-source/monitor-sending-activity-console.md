# Monitoring your sending statistics using the Amazon SES console<a name="monitor-sending-activity-console"></a>

You can monitor the number of emails sent from your account, as well as the percentage of your sending quota that's been used, directly from the Amazon SES console's Account dashboard in the *Daily email usage* section\. 

Also from the Account dashboard, you can also monitor your bounce and complaint rates in the *Sending statistics* section\. This shouldn’t be confused with similar rates shown on the console’s Reputation metrics page\. It's important to understand the difference between these two sets of bounce and complaint rates:
+ **Reputation metrics page** – only your current bounce and complaint rates are shown based on the latest data point received from calculating an average metric based on your overall historic bounce/complaint rate including up to the present time\.
+ **Account dashboard page** – based on the date range selected, you can view what the bounce and complaint rates were in the past showing the metric progression of change leading up to the present time\.

For example, if the rate was 2% yesterday and is 1% now, on the Reputation metrics page, you'll only see the current rate of 1%, but on the Account dashboard page, the Sending statistics graphs will plot the charted progression showing a rate of 2% for yesterday and 1% for today\.

The Account dashboard's *Sending statistics* section is comprised of graphs that show the progression of four essential metrics in a time\-ordered set of data points representing the values of a monitored event type producing statistics for the selected date range using an aggregation period of *1 hour*\. You can select a data range with start values from `Last 1 day` to `Last 14 days` to filter the charts below:
+ **Sends** – the sum of successful email send requests\.
+ **Rejects** – the average rate of rejected send requests by SES using the metric math `Rejects/Sends * 100`\.
+ **Bounces and Complaints** – the average rate derived from your overall historic sender reputation metrics, *Reputation\.BounceRate* and *Reputation\.ComplaintRate*, but showing the progression of those metrics in time for the date range selected\.

Each of these charts contain a **View in CloudWatch** button that will open the respective metric in the Amazon CloudWatch console allowing detailed data to be viewed, customized metric math performed, and [the creation of alarms in CloudWatch](reputationdashboard-cloudwatch-alarm.md)\.

**To view emails sent and sending quota used**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, choose **Account dashboard**\. Your usage statistics are shown in the **Daily email usage** section\.

**To view count of sends, rates of rejects, bounces, and complaints**

1. In the navigation pane, choose **Account dashboard**\.

1. In the **Sending statistics** section, use the **Date range** dropdown to select a start value for a date range to filter the four charts directly below the **Sending statistics** section\.

1. Based on the date range selected, you can view what the counts and rates were in the past showing the metric progression of change leading up to the present time\.

1. In any of the charts, choose the **View in CloudWatch** button to open the respective metric in the **Amazon CloudWatch** console where you can view detailed data, perform customized metric math, and [create monitoring alarms in CloudWatch](reputationdashboard-cloudwatch-alarm.md)\.