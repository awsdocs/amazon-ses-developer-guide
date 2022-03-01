# Step 2: Connect to Your Amazon Redshift Cluster<a name="event-publishing-redshift-cluster-connect"></a>

Now you will connect to your cluster by using a SQL client tool\. For this tutorial, you use the SQL Workbench/J client that you installed in the [prerequisites section](event-publishing-redshift-prerequisites.md)\.

Complete this section by performing the following steps:
+ [Getting Your Connection String](#event-publishing-redshift-cluster-connect-connection-string)
+ [Connecting to Your Cluster From SQL Workbench/J](#event-publishing-redshift-cluster-connect-profile)

## Getting Your Connection String<a name="event-publishing-redshift-cluster-connect-connection-string"></a>

The following procedure shows how to get the connection string that you will need to connect to your Amazon Redshift cluster from SQL Workbench/J\.

**To get your connection string**

1. In the Amazon Redshift console, in the navigation pane, choose **Clusters**\.

1. To open your cluster, choose your cluster name\.

1. On the **Configuration** tab, under **Cluster Database Properties**, copy the JDBC URL of the cluster\. 
**Note**  
The endpoint for your cluster is not available until the cluster is created and in the available state\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_redshift_url.png)



## Connecting to Your Cluster From SQL Workbench/J<a name="event-publishing-redshift-cluster-connect-profile"></a>

The following procedure shows how to connect to your cluster from SQL Workbench/J\. This procedure assumes that you installed SQL Workbench/J on your computer as described in [Prerequisites](event-publishing-redshift-prerequisites.md)\.

**To connect to your cluster from SQL Workbench/J**

1. Open SQL Workbench/J\.

1. Choose **File**, and then choose **Connect window**\.

1. Choose the **Create a new connection profile** button\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_redshift_new_connection.png)

1. In the **New profile** text box, type a name for the profile\.

1. At the bottom of the window, on the left, choose **Manage Drivers**\.

1. In the **Manage Drivers** dialog box, choose the **Create a new entry** button, and then add the driver as follows\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_redshift_new_entry.png)

   1. In the **Name** box, type a name for the driver\.

   1. Next to **Library**, choose the folder icon\.

   1. Navigate to the location of the driver you downloaded in [Configure a JDBC Connection](https://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-connection.html), select the driver, and then choose **Open**\.

   1. Choose **OK**\.

      You will be taken back to the **Select Connection Profile** dialog box\.

1. For **Driver**, choose the driver that you just added\.

1. For **URL**, paste the JDBC URL that you copied from the [Amazon Redshift console](https://console.aws.amazon.com/redshift/)\.

1. For **Username**, type the username that you chose when you [set up the Amazon Redshift cluster](event-publishing-redshift-cluster.md)\.

1. For **Password**, type the password that you chose when you set up the Amazon Redshift cluster\.

1. Select **Autocommit**\. 

1. To test the connection, choose **Test**\.
**Note**  
If the connection attempt times out, you might need to add your IP address to the security group that allows incoming traffic from IP addresses\. For more information, see [The Connection Is Refused or Fails](https://docs.aws.amazon.com/redshift/latest/mgmt/connecting-refusal-failure-issues.html) in the *Amazon Redshift Database Developer Guide*\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_redshift_connection.png)

1. On the top menu bar, choose the **Save profile list** button\.

1. Choose **OK**\.

   SQL Workbench/J will connect to your Amazon Redshift cluster\.

## Next Step<a name="event-publishing-redshift-cluster-connect-next-step"></a>

[Step 3: Create a Database Table](event-publishing-redshift-table.md)
