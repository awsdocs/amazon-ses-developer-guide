# Part 8: Configure Triggers in Amazon CloudWatch<a name="dashboardtriggercloudwatch"></a>

After you validate that the Lambda function is working properly, you can use Amazon CloudWatch to schedule the function to run every day\. Each time the Lambda function runs, you receive an email that includes a link to a web page stored in an Amazon S3 bucket\. When you click the link, you see a dashboard that displays information about the numbers of bounces and complaints that were received in the previous 24\-hour period\.

**To configure trigger conditions in Amazon CloudWatch**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation bar, under **Events**, choose **Rules**\.

1. Choose **Create rule**\.

1. Under **Event Source**, choose **Schedule**\.

1. For **Fixed rate of**, choose **1 Day**\.

1. Under **Targets**, choose **Add target**\.

1. For **Function**, choose the Lambda function you created in [Part 6: Create an AWS Lambda Function](dashboardcreatelambdafunction.md), and then choose **Configure details**\.

1. Under **Rule definition**, for **Name**, type a name for the rule, and then choose **Create rule**\.