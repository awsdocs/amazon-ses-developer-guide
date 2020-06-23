# Monitoring your Amazon SES sender reputation<a name="monitor-sender-reputation"></a>

Amazon SES actively tracks several metrics that may cause your reputation as a sender to be damaged, or that could cause your email delivery rates to decline\. Two important metrics that we consider in this process are the bounce and complaint rates for your account\. If the bounce or complaint rates for your account are too high, we might place your account under review or pause your account's ability to send email\.

Because your bounce and complaint rate are so important to the health of your account, Amazon SES includes a reputation dashboard that you can use to track these metrics\. The reputation dashboard can also display information about factors unrelated to bounces or complaints that could damage your sender reputation\. For example, if you send email to a known [spamtrap](https://en.wikipedia.org/wiki/Spamtrap), you will see a message on this dashboard\.

This section contains information about accessing the reputation dashboard, interpreting the information it contains, and setting up systems to actively notify you of factors that could impact your sender reputation\.

**Topics**
+ [Using the reputation dashboard to track bounce and complaint rates](reputation-dashboard-dg.md)
+ [Reputation dashboard messages](reputationdashboardmessages.md)
+ [Creating reputation monitoring alarms using CloudWatch](reputationdashboard-cloudwatch-alarm.md)
+ [Automatically pausing email sending](monitoring-sender-reputation-pausing.md)