# Part 4: Create AWS Identity and Access Management Policies and Roles<a name="dashboardconfigureIAM"></a>

To ensure the security of your AWS account, you must create an AWS Identity and Access Management \(IAM\) policy and role\. The policy and role define the ways that the components of this solution can interact with each other\. This procedure describes how to configure these policies and roles\.

**To create a new IAM policy and role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation bar, choose **Policies**\.

1. Choose **Create policy**\.

1. On the **JSON** tab, paste the following code into the editor:

   ```
   {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "AllowSendEmail",
              "Effect": "Allow",
              "Action": [
                  "ses:SendEmail"
              ],
              "Resource": [
                  "*"
              ]
          },
          {
              "Sid": "s3allow",
              "Effect": "Allow",
              "Action": [
                  "s3:PutObject",
                  "s3:PutObjectAcl"
              ],
              "Resource": [
                  "arn:aws:s3:::BUCKET_NAME/*"
              ]
          },
          {
              "Sid": "AllowQueuePermissions",
              "Effect": "Allow",
              "Action": [
                  "sqs:ChangeMessageVisibility",
                  "sqs:ChangeMessageVisibilityBatch",
                  "sqs:DeleteMessage",
                  "sqs:DeleteMessageBatch",
                  "sqs:GetQueueAttributes",
                  "sqs:GetQueueUrl",
                  "sqs:ReceiveMessage"
              ],
              "Resource": [
                  "SQS_QUEUE_ARN"
              ]
          },
          {
              "Effect": "Allow",
              "Action": [
                  "logs:CreateLogGroup",
                  "logs:CreateLogStream",
                  "logs:PutLogEvents",
                  "logs:DescribeLogStreams"
              ],
              "Resource": [
                  "arn:aws:logs:*:*:*"
              ]
          }
      ]
   }
   ```

   In the pasted code, change the following attributes:

   + Replace *BUCKET\_NAME* with the name of the Amazon S3 bucket you created in [Part 3: Create an Amazon Simple Storage Service Bucket](dashboardcreateS3bucket.md)\.

   + Replace *SQS\_QUEUE\_ARN* with the ARN of the Amazon SQS queue you created in [Part 2: Create a Queue in Amazon Simple Queue Service](dashboardcreateSQSqueue.md)\.
**Tip**  
You can find the ARN of the Amazon SQS queue by completing the following steps:  
In the Amazon SQS console, choose the queue you created in [Part 2: Create a Queue in Amazon Simple Queue Service](dashboardcreateSQSqueue.md)\.
In the **Details** section, copy the value shown next to **ARN**\. The format of the ARN should resemble the following example:  
`arn:aws:sqs:us-east-1:999623213###:sample-queue-name`

1. Choose **Review policy**\.

1. On the **Review policy** screen, complete the following sections:

   + For **Name**, type a name for the policy\.

   + For **Description**, type a brief description of the policy\.

1. Choose **Create policy**\.

1. In the navigation bar, choose **Roles**\.

1. Choose **Create role**\.

1. Under **Select type of trusted entity**, choose **Lambda**, and then choose **Next: Permissions**\.

1. On the **Attach permissions policies** screen, check the box next to the name of the policy you just created, and then choose **Next: Review**\.

1. On the **Review** screen, for **Role name**, type a name for the role, and then choose **Create role**\.

1. Proceed to [Part 5: Configure Bounce and Complaint Notifications in Amazon SES](dashboardconfigureSESnotifications.md)\.