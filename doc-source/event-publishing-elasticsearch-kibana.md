# Step 5: Visualize Data in Kibana<a name="event-publishing-elasticsearch-kibana"></a>

Now that you have published some Amazon SES email sending events to Amazon Elasticsearch Service \(Amazon ES\) by sending emails with your configuration set and message tags, you can visualize the events using Kibana, a web interface for Elasticsearch\.

**Note**  
Kibana is a third\-party application, and isn't developed or supported by Amazon Web Services\. The procedures in this section are provided for informational purposes only, and are subject to change without notice\.

This section shows how to find your email sending events in Kibana, graph your email sending events by event type, and find the email addresses that bounced\. These exercises are useful because monitoring your bounces and complaints is an important part of maintaining your mailing list\.
+ [Viewing Your Email Sending Events](#event-publishing-elasticsearch-kibana-view-raw)
+ [Graphing Your Email Sending Events by Type](#event-publishing-elasticsearch-kibana-graph)
+ [Finding Recipient Addresses That Bounced](#event-publishing-elasticsearch-kibana-bounces)

For Kibana documentation and tutorials, see the [Kibana User Guide](https://www.elastic.co/guide/en/kibana/current/index.html)\.

## Viewing Your Email Sending Events<a name="event-publishing-elasticsearch-kibana-view-raw"></a>

The following procedure shows how to go to the Kibana user interface from the Amazon ES console\. It then shows how to view the email sending events associated with the index you defined when you set up your Amazon Kinesis Data Firehose delivery stream in [Step 2: Create a Kinesis Data Firehose Delivery Stream](event-publishing-elasticsearch-firehose-stream.md)\.

**To view your raw email sending events in Kibana**

1. Sign in to the AWS Management Console and open the Amazon Elasticsearch Service console at [https://console\.aws\.amazon\.com/es/](https://console.aws.amazon.com/es/)\.

1. Under **My Elasticsearch domains**, choose the domain you created in [Step 1: Create an Amazon ES Cluster](event-publishing-elasticsearch-cluster.md)\.

1. Choose the Kibana link\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_elasticsearch_kibana_link.png)

   You will be directed to the Kibana web interface\.

1. On the **Configure an index pattern** page, clear the **Index contains time\-based events** check box\.

1. Under **Index name or pattern**, verify that `holiday-sale`, the index you created in [Step 1: Create an Amazon ES Cluster](event-publishing-elasticsearch-cluster.md), is present\. If it is not present, type `holiday-sale*` into the field, and then choose **Create**\.
**Note**  
If the **Create** button does not appear, try adding an asterisk to the end of the index pattern\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_elasticsearch_kibana_index.png)

1. Choose the **Discover** tab on the top menu\.

1. In the search box below the **Discover** tab, put the cursor after the asterisk \(\*\), and then press Enter\.

   Kibana will display a list of all of your email sending events\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_elasticsearch_kibana_discover.png)

## Graphing Your Email Sending Events by Type<a name="event-publishing-elasticsearch-kibana-graph"></a>

For insight into the overall health of your email campaign, you can visually compare the number of problematic outcomes \(bounces, complaints, and rejected emails\) you have received across your campaign\. The following procedure shows how to set up a vertical bar chart that displays the count of each type of email sending event\.

**To graph your email sending events by type**

1. Choose the **Visualize** tab on the top menu\.

1. Choose **Vertical bar chart**\.

1. Choose **From a new search**\. If prompted for an index pattern, choose `holiday-sale*`\.

1. On the metrics pane, next to **Y\-Axis**, ensure that the metric is set to **Count**\.

1. In the **Buckets** pane, choose **X\-Axis**\.

1. For **Aggregation**, choose **Terms**\. *Terms* refers to the fields in your JSON documents in your index\.

1. For **Field**, under **String**, choose **eventType**\. Leave the rest of the fields at their default values\.

1. Next to **Options**, choose the play button\. Your bar chart comparing the event types will display on the screen\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_elasticsearch_kibana_graph_all.png)

1. To save your visualization, choose the save icon from the group of icons to the right of the search bar\.

1. Type a title such as **All Event Types**, and then choose **Save**\.

## Finding Recipient Addresses That Bounced<a name="event-publishing-elasticsearch-kibana-bounces"></a>

Now that you have a visualization of the health of your overall email campaign, you can go back to the raw data and find out which recipient addresses bounced, as described in the following procedure\.

**To view the email addresses that bounced**

1. Go to the **Discover** tab on the top menu\.

   You will see a list of your raw event records\.
**Note**  
If the main window reports that there are zero search results, enter `*` in the search bar, and then press Enter\.

1. In the left pane, under **Available Fields**, hover over **eventType**, and then choose the **add** button that appears next to it\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_elasticsearch_kibana_add.png)

1. In the main window, hover over the **eventType** column heading, and then choose the arrow to sort the event types by name\.

   The bounce events will move to the top of the list\.
**Note**  
There might be a short delay before the events are resorted\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_elasticsearch_kibana_sort.png)

1. In the left pane, under **Available Fields**, hover over **bounce\.bouncedRecipients**, and then choose the **add** button that appears next to it\.

   In the main window, you will see the recipient address and bounce reason for each bounce event\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_elasticsearch_kibana_bounces.png)