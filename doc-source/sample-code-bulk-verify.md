# Verify multiple email addresses<a name="sample-code-bulk-verify"></a>

If you are migrating to Amazon SES from another email\-sending solution, you may already have a long list of email addresses that you want to use to send email\. The Python script in this example accepts a JSON\-formatted list of email addresses as an input\. The following example shows the structure of the input file:

```
[
  {
    "email":"carlos.salazar@example.com"
  },
  {
    "email":"mary.major@example.co.uk"
  },
  {
    "email":"wei.zhang@example.cn"
  }
]
```

The following script reads the input file and attempts to validate all of the email addresses contained in the file\. This code example assumes that you have installed the AWS SDK for Python \(Boto\), and that you have created a shared credentials file\. For more information about creating a shared credentials file, see [Create a shared credentials file](create-shared-credentials-file.md)\.

```
 1. import json     #Python standard library
 2. import boto3    #sudo pip install boto3
 3. from botocore.exceptions import ClientError
 4. 
 5. # The full path to the file that contains the identities to be verified. 
 6. # The input file must be JSON-formatted. See
 7. # https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sample-code-bulk-verify.html 
 8. # for a sample input file.
 9. FILE_INPUT = '/path/to/identities.json'
10. 
11. # If necessary, replace us-west-2 with the AWS Region you're using for Amazon SES.
12. AWS_REGION = "us-west-2"
13. 
14. # Create a new SES resource specify a region.
15. client = boto3.client('ses',region_name=AWS_REGION)
16. 
17. # Read the file that contains the identities to be verified.
18. with open(FILE_INPUT) as data_file:
19.     data = json.load(data_file)
20. 
21. # Iterate through the array from the input file. Each time an object named
22. # 'email' is found, run the verify_email_identity operation against the value 
23. # of that object.
24. for i in data:
25.     try:
26.         response = client.verify_email_identity(
27.             EmailAddress=i['email']
28.     )
29.     # Display an error if something goes wrong.	
30.     except ClientError as e:
31.         print(e.response['Error']['Message'])
32.     # Otherwise, show the request ID of the verification message.
33.     else:
34.         print('Verification email sent to ' + i['email'] + '. Request ID: ' + 
35.               response['ResponseMetadata']['RequestId'])
```
