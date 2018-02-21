# Step 1: Create an Amazon Redshift Cluster<a name="event-publishing-redshift-cluster"></a>

To create an Amazon Redshift cluster, go the [Amazon Redshift console](https://console.aws.amazon.com/redshift/) and choose **Launch Cluster**\. A wizard guides you through choosing options for your cluster, and it provides default values for most options\.

For this simple tutorial, type a cluster name and password, and then you can use all of the default values\. You do not need to set any values specific to Amazon SES event publishing\.

**Important**  
The cluster that you deploy for this tutorial will run in a live environment\. As long as it is running, it will accrue charges to your AWS account\. To avoid unnecessary charges, you should delete your cluster when you are done with it\. For pricing information, go to the [Amazon Redshift pricing page](https://aws.amazon.com/redshift/pricing/)\. 

## Next Step<a name="event-publishing-redshift-cluster-next-step"></a>

[Step 2: Connect to Your Amazon Redshift Cluster](event-publishing-redshift-cluster-connect.md)