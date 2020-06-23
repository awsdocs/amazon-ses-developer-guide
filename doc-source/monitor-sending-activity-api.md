# Monitoring your usage statistics using the Amazon SES API<a name="monitor-sending-activity-api"></a>

The Amazon SES API provides the `GetSendStatistics` operation, which returns information about your service usage\. We recommend that you check your sending statistics regularly, so that you can make adjustments if needed\.

When you call the `GetSendStatistics` operation, you receive a list of data points representing the last two weeks of your sending activity\. Each data point in this list represents 15 minutes of activity and contains the following information for that period:
+ The number of hard bounces
+ The number of complaints
+ The number of delivery attempts \(corresponds to the number of emails you have sent\)
+ The number of rejected send attempts
+ A timestamp for the analysis period

For a complete description of the `GetSendStatistics` operation, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/GetSendStatistics.html)\.

In this section, you will find the following topics:
+ [Calling the `GetSendStatistics` API operation using the AWS CLI](#monitor-sending-activity-api-cli)
+ [Calling the `GetSendStatistics` operation programmatically](#monitor-sending-activity-api-sdk)

## Calling the `GetSendStatistics` API operation using the AWS CLI<a name="monitor-sending-activity-api-cli"></a>

The easiest way to call the `GetSendStatistics` API operation is to use the [AWS Command Line Interface](https://aws.amazon.com/cli) \(AWS CLI\)\.

**To call the `GetSendStatistics` API operation using the AWS CLI**

1. If you have not already done so, install the AWS CLI\. For more information, see "[Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)" in the *AWS Command Line Interface User Guide*\.

1. If you have not already done so, configure the AWS CLI to use your AWS credentials\. For more information, see "[Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)" in the *AWS Command Line Interface User Guide*\.

1. At the command line, type **aws ses get\-send\-statistics**

   If the AWS CLI is properly configured, you see a list of sending statistics in JSON format\. Each JSON object includes aggregated sending statistics for a 15\-minute period\.

## Calling the `GetSendStatistics` operation programmatically<a name="monitor-sending-activity-api-sdk"></a>

You can also call the `GetSendStatistics` operation using the AWS SDKs\. This section includes code examples for the AWS SDKs for Go, PHP, Python, and Ruby\. Choose one of the following links to view code examples for that language:
+ [Code example for the AWS SDK for Go](#code-example-getsendstatistics-golang)
+ [Code example for the AWS SDK for PHP](#code-example-getsendstatistics-php)
+ [Code example for the AWS SDK for Python \(Boto\)](#code-example-getsendstatistics-python)
+ [Code example for the AWS SDK for Ruby](#code-example-getsendstatistics-ruby)

**Note**  
These code examples assume that you have created an AWS shared credentials file that contains your AWS Access Key ID, your AWS Secret Access Key, and your preferred AWS Region\. For more information, see [Create a shared credentials file](create-shared-credentials-file.md)\.

### Calling `GetSendStatistics` using the AWS SDK for Go<a name="code-example-getsendstatistics-golang"></a>

```
 1. package main
 2.     
 3. import (
 4.     "fmt"
 5.     
 6.     //go get github.com/aws/aws-sdk-go/...
 7.     "github.com/aws/aws-sdk-go/aws"
 8.     "github.com/aws/aws-sdk-go/aws/session"
 9.     "github.com/aws/aws-sdk-go/service/ses"
10.     "github.com/aws/aws-sdk-go/aws/awserr"
11. )
12.     
13. const (
14.     // Replace us-west-2 with the AWS Region you're using for Amazon SES.
15.     AwsRegion = "us-west-2"
16. )
17.     
18. func main() {
19.     
20.     // Create a new session and specify an AWS Region.
21.     sess, err := session.NewSession(&aws.Config{
22.         Region:aws.String(AwsRegion)},
23.     )
24.     
25.     // Create an SES client in the session.
26.     svc := ses.New(sess)
27.     input := &ses.GetSendStatisticsInput{}
28.     
29.     result, err := svc.GetSendStatistics(input)
30.     
31.     // Display error messages if they occur.
32.     if err != nil {
33.         if aerr, ok := err.(awserr.Error); ok {
34.             switch aerr.Code() {
35.             default:
36.                 fmt.Println(aerr.Error())
37.             }
38.         } else {
39.             // Print the error, cast err to awserr.Error to get the Code and
40.             // Message from an error.
41.             fmt.Println(err.Error())
42.         }
43.         return
44.     }
45.     
46.     fmt.Println(result)
47. }
```

### Calling `GetSendStatistics` using the AWS SDK for PHP<a name="code-example-getsendstatistics-php"></a>

```
 1. <?php
 2. 
 3. // Replace path_to_sdk_inclusion with the path to the SDK as described in 
 4. // http://docs.aws.amazon.com/aws-sdk-php/v3/guide/getting-started/basic-usage.html
 5. define('REQUIRED_FILE','path_to_sdk_inclusion');
 6.                                                   
 7. // Replace us-west-2 with the AWS Region you're using for Amazon SES.
 8. define('REGION','us-west-2'); 
 9. 
10. require REQUIRED_FILE;
11. 
12. use Aws\Ses\SesClient;
13. 
14. $client = SesClient::factory(array(
15.     'version'=> 'latest',     
16.     'region' => REGION
17. ));
18. 
19. try {
20.      $result = $client->getSendStatistics([]);
21. 	 echo($result);
22. } catch (Exception $e) {
23.      echo($e->getMessage()."\n");
24. }
25. 
26. ?>
```

### Calling `GetSendStatistics` using the AWS SDK for Python \(Boto\)<a name="code-example-getsendstatistics-python"></a>

```
 1. import boto3 #pip install boto3
 2. import json
 3. from botocore.exceptions import ClientError
 4. 
 5. client = boto3.client('ses')
 6. 
 7. try:
 8.     response = client.get_send_statistics(
 9. )
10. except ClientError as e:
11.     print(e.response['Error']['Message'])
12. else:
13.     print(json.dumps(response, indent=4, sort_keys=True, default=str))
```

### Calling `GetSendStatistics` using the AWS SDK for Ruby<a name="code-example-getsendstatistics-ruby"></a>

```
 1. require 'aws-sdk' # gem install aws-sdk
 2. require 'json'
 3. 
 4. # Replace us-west-2 with the AWS Region you're using for Amazon SES.
 5. awsregion = "us-west-2"
 6. 
 7. # Create a new SES resource and specify a region
 8. ses = Aws::SES::Client.new(region: awsregion)
 9. 
10. begin
11. 
12.   resp = ses.get_send_statistics({
13.   })
14.   puts JSON.pretty_generate(resp.to_h)
15. 
16. # If something goes wrong, display an error message.
17. rescue Aws::SES::Errors::ServiceError => error
18.   puts error
19. 
20. end
```