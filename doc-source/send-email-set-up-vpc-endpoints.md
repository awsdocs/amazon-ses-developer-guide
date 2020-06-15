# Setting up VPC endpoints with Amazon SES<a name="send-email-set-up-vpc-endpoints"></a>

Many Amazon SES customers have corporate policies in place that limit the ability of their internal systems to connect to the public internet\. These policies prevent these customers from using the public Amazon SES endpoints\.

To work within these restrictions, you can use Amazon Virtual Private Cloud \(Amazon VPC\)\. With Amazon VPC, you can deploy AWS resources into a virtual network that exists in an isolated area of the AWS Cloud\. For more information about Amazon VPC, see the [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)\.

To use Amazon SES with Amazon VPC, you first have to create an Amazon EC2 instance in your organization's VPC\. You can then connect to this instance and use it to send email through Amazon SES\. This section contains instructions for configuring your Amazon EC2 instance and creating an Amazon VPC endpoint for Amazon SES\.

## Prerequisites<a name="send-email-set-up-vpc-endpoints-prereqs"></a>

Before you complete the procedure in this section, you have to complete the following steps:
+ Create a Virtual Private Cloud\. For procedures, see [Getting started with IPv4 for Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/getting-started-ipv4.html)\. 
+ Launch an Amazon EC2 instance in your VPC\. For more information, see [Launching an EC2 instance into your default VPC](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html#launching-into)\.

## Setting up Amazon SES in Amazon VPC<a name="send-email-set-up-vpc-endpoints-procedure"></a>

The process of setting up a VPC endpoint to use with Amazon SES consists of a few separate steps\. First, you have to identify the private IP address of the Amazon EC2 instance that you want to use with the VPC endpoint\. Next, you create a security group that allows the instance to communicate with SMTP ports\. After that, you create a VPC endpoint for Amazon SES\. Finally, you test the connection to the VPC endpoint to ensure that it's configured properly\.

### Step 1: Find the Private IP Address of Your Amazon EC2 Instance<a name="send-email-set-up-vpc-endpoints-procedure-step-1"></a>

To set up an Amazon EC2 instance to use an Amazon SES VPC endpoint, you first have to find the private IP of the instance\. You use this IP address in a later step\.

**To find the private IP of an Amazon EC2 instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Instances**, choose **Instances**\.

1. In the list of Amazon EC2 instances, choose the instance that you want to use to connect to the VPC endpoint\.

1. In the detail pane at the bottom of the screen, on the **Description** tab, copy the IP address next to **Private IP**\.

### Step 2: Create the Security Group<a name="send-email-set-up-vpc-endpoints-procedure-step-2"></a>

In Amazon EC2, a *security group* lets you control inbound and outbound communications to and from your VPC\. In this step, you create a security group that lets the Amazon EC2 instance communicate with SMTP endpoints\.

**To create the security group**

1. In the navigation pane of the Amazon EC2 console, under **Network & Security**, choose **Security Groups**\.

1. Choose **Create security group**\.

1. Under **Basic details**, do the following:
   + For **Security group name**, enter a unique name that identifies the security group\. 
   + For **Description**, enter some text that describes the purpose of the security group\. 
   + For **VPC**, choose the VPC that you want to use Amazon SES in\.

   When you finish, the **Basic details** section resembles the example in the following image\.  
![\[\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/send-email-set-up-vpc-endpoint-1.png)

1. Under **Inbound rules**, choose **Add rule**\. 

1. Under **Inbound rule 1**, do the following:
   + For **Type**, choose **Custom TCP**\.
   + For **Port range**, enter the port number that you want to use to send email\. You can use any of the following port numbers: **25**, **465**, **587**, **2465**, or **2587**\.
   + For **Source type**, choose **Custom**\.
   + For **Source**, enter the private IP of your Amazon EC2 instance \(that is, the address that you found earlier\)\.

1. \(Optional\) If you want to add an inbound rule for additional ports, choose **Add rule** again\. Then, repeat the preceding step to add additional ports\. You can create rules for any or all of the port numbers listed in the preceding step\.

1. When you finish, choose **Create security group**\.

### Step 3: Create the VPC endpoint<a name="send-email-set-up-vpc-endpoints-procedure-step-3"></a>

In Amazon VPC, a *VPC endpoint* lets you connect your VPC to supported AWS services\. In this case, you configure Amazon VPC so that your Amazon EC2 security group can connect to Amazon SES\.

**To create the security group**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. Under **Virtual Private Cloud**, choose **Endpoints**\.

1. Choose **Create Endpoint**\.

1. On the **Create Endpoint** page, for **Service category**, choose **AWS services**\.

1. Under **Service Name**, use the search box to search for "email", as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/send-email-set-up-vpc-endpoint-3.png)

   Choose the `email-smtp` service for your current AWS Region\. 

1. For **VPC**, choose the Virtual Private Cloud that you want to use\.

1. Under **Security group**, choose the security group that you created earlier, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/send-email-set-up-vpc-endpoint-4.png)

1. Choose **Create endpoint**\. Wait approximately 5 minutes while Amazon VPC creates the endpoint\. When the endpoint is ready to use, the value in the **Status** column changes to "available", as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/send-email-set-up-vpc-endpoint-5.png)

### Step 4: Test the connection to the VPC endpoint<a name="send-email-set-up-vpc-endpoints-procedure-step-4"></a>

When you complete the process of configuring the VPC endpoint, you should test the connection to ensure that the VPC endpoint is configured properly\. You can test the connection by using command\-line tools that are included with most operating systems\.

**To test the connection to the VPC endpoint**

1. Connect to your Amazon EC2 instance\. 

   For information about connecting to Linux instances, see [Connect to your Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

   For information about connecting to Windows instances, see [Getting started](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows) in the *Amazon EC2 User Guide for Windows Instances*\.

1. Send a test email by completing the procedure in [Using the Command Line to Send Email Using the Amazon SES SMTP Interface](send-email-smtp-client-command-line.md#send-email-using-openssl)\.
**Note**  
You have to verify an email address or domain before you can send email through Amazon SES\. For more information about verifying identities, see [Verifying identities in Amazon SES](verify-addresses-and-domains.md)\.