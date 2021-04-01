# Step 3: Graph Email Sending Events<a name="event-publishing-cloudwatch-tutorial-graph"></a>

Now that you have published some Amazon SES email sending events to CloudWatch by sending emails with your configuration set and message tags, you can graph metrics for those events using the CloudWatch console\. 

**To graph email sending event metrics**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Metrics**\.

1. In the **All metrics** tab, choose **SES**\.
**Tip**  
You can also type `SES` into the search field\.

1. Choose the value source that you specified in [Adding a CloudWatch Event Destination](event-publishing-cloudwatch-tutorial-configuration-set.md#event-publishing-cloudwatch-tutorial-configuration-set-add-destination)\. For example, if you specified the message tag "category:books" as the value source, choose **category**\.

1. Choose the metric that you want to view\. A graph appears in the details pane\.
