# Step 1: Set up a Configuration Set<a name="event-publishing-cloudwatch-tutorial-configuration-set"></a>

To set up Amazon SES to publish your email sending events to Amazon CloudWatch, you first create a configuration set, and then you add a CloudWatch event destination to the configuration set\. This section shows how to accomplish those tasks\.

If you already have a configuration set, you can add a CloudWatch destination to your existing configuration set\. In this case, skip to [Adding a CloudWatch Event Destination](#event-publishing-cloudwatch-tutorial-configuration-set-add-destination)\.

## Creating a Configuration Set<a name="event-publishing-cloudwatch-tutorial-configuration-set-create"></a>

The following procedure shows how to create a configuration set\.

**To create a configuration set**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. In the content pane, choose **Create Configuration Set**\.

1. Type a name for the configuration set, and then choose **Create Configuration Set**\.

1. Choose **Close**\.

## Adding a CloudWatch Event Destination<a name="event-publishing-cloudwatch-tutorial-configuration-set-add-destination"></a>

The following procedure shows how to add a CloudWatch event destination to the configuration set you created\.

**To add a CloudWatch event destination to a configuration set**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. Choose the configuration set you created in the previous procedure\.

1. For **Add Destination**, choose **Select a destination type**, and then choose **CloudWatch**\.

1. For **Name**, type a name for the event destination\.

1. For **Event types**, choose **Bounce** and **Complaint**\.

1. Select **Enabled**\.

1. For **Value Source**, choose **Message Tag**\.

1. For **Dimension Name**, type `campaign`\.

1. For **Default Value**, type `unknown`\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_cloudwatch_destination.png)

1. Choose **Save**\.

1. To exit the **Edit Configuration Set** page, use the back button of your browser\.