# Step 1: Create an Amazon ES Cluster<a name="event-publishing-elasticsearch-cluster"></a>

Before you set up Amazon Kinesis Data Firehose to publish your Amazon SES email sending events to Amazon Elasticsearch Service \(Amazon ES\), you must create an Amazon ES cluster\. This section shows how to create an Amazon ES cluster using the Amazon ES console\.

For the simplicity of this tutorial, we choose basic options\. For information about all available options, see the [Amazon Elasticsearch Service Developer Guide](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/what-is-amazon-elasticsearch-service.html)\.

**Important**  
The cluster that you deploy for this tutorial will run in a live environment\. As long as it is running, it will accrue charges to your AWS account\. To avoid unnecessary charges, you should delete your cluster when you are done with it\. For pricing information, go to the [Amazon Elasticsearch Service pricing page](https://aws.amazon.com/elasticsearch-service/pricing/)\. 

**To create an Amazon ES cluster**

1. Sign in to the AWS Management Console and open the Amazon Elasticsearch Service console at [https://console\.aws\.amazon\.com/es/](https://console.aws.amazon.com/es/)\.

1. In the Amazon ES console, choose **Get started**\.

1. On the **Define domain** page, under **Domain Name**, type a name for your Amazon ES domain\.

1. Under **Version**, leave the **Elasticsearch version** field at its default value\.

1. Choose **Next**\.

1. On the **Configure cluster** page, under **Node configuration**, choose the following options\.
   + **Instance count** – Type **1**\.
   + **Instance type** – Choose **t2\.micro\.elasticsearch \(Free tier eligible\)**\.
   + **Enable dedicated master** – Do not enable this option\.
   + **Enable zone awareness** – Do not enable this option\.

1. Under **Storage configuration**, choose the following options\.
   + **Storage type** – Choose **EBS**\. For the EBS settings, choose **EBS volume type** of **General Purpose \(SSD\)** and **EBS volume size ** of **10**\.
   + **Automated snapshot start hour** – Choose **Automated snapshots start hour 00:00 UTC \(default\)**\.

1. Choose **Next**\.

1. On the **Set up access policy** page, for **Set the domain access policy to**, choose **Allow open access to the domain**\.
**Important**  
This setting simplifies testing but is **not recommended for production environments**\. For information about configuring access policies, see [Configuring Access Policies](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-createupdatedomains.html#es-createdomain-configure-access-policies) in the *Amazon Elasticsearch Service Developer Guide*\.

1. Choose **Next**\.

1. On the **Review** page, review your settings, and then choose **Confirm and create**\.
**Note**  
The cluster will take up to ten minutes to deploy\.

## Next Step<a name="event-publishing-elasticsearch-cluster-next-step"></a>

[Step 2: Create a Kinesis Data Firehose Delivery Stream](event-publishing-elasticsearch-firehose-stream.md)