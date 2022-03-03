# Create and manage rules and filters for Amazon SES using an AWS SDK<a name="example_ses_Scenario_ReceiptRulesFilters_section"></a>

The following code example shows how to create and manage rules and filters for Amazon SES that affect how incoming emails are handled\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
Use functions from the API examples section to create and manage rules and filters\.  

```
def usage_demo():
    print('-'*88)
    print("Welcome to the Amazon Simple Email Service (Amazon SES) receipt rules "
          "and filters demo!")
    print('-'*88)

    logging.basicConfig(level=logging.INFO, format='%(levelname)s: %(message)s')

    ses_receipt = SesReceiptHandler(boto3.client('ses'), boto3.resource('s3'))
    filter_name = 'block-self'
    rule_set_name = 'doc-example-rule-set'
    rule_name = 'copy-mail-to-bucket'
    email = 'example@example.org'
    bucket_name = f'doc-example-bucket-{time.time_ns()}'
    prefix = 'example-emails/'

    current_ip_address = request.urlopen(
        'http://checkip.amazonaws.com').read().decode('utf-8').strip()
    print(f"Adding a filter to block email from the current IP address "
          f"{current_ip_address}.")
    ses_receipt.create_receipt_filter(filter_name, current_ip_address, False)
    filters = ses_receipt.list_receipt_filters()
    print("Current filters now in effect are:")
    print(*filters, sep='\n')
    print("Removing filter.")
    ses_receipt.delete_receipt_filter(filter_name)

    print(f"Creating a rule set and adding a rule to copy all emails received by "
          f"{email} to Amazon S3 bucket {bucket_name}.")
    print(f"Creating bucket {bucket_name} to hold emails.")
    bucket = ses_receipt.create_bucket_for_copy(bucket_name)
    ses_receipt.create_receipt_rule_set(rule_set_name)
    ses_receipt.create_s3_copy_rule(
        rule_set_name, rule_name, [email], bucket.name, prefix)
    rule_set = ses_receipt.describe_receipt_rule_set(rule_set_name)
    print(f"Rule set {rule_set_name} looks like this:")
    pprint(rule_set)
    print(f"Deleting rule {rule_name} and rule set {rule_set_name}.")
    ses_receipt.delete_receipt_rule(rule_set_name, rule_name)
    ses_receipt.delete_receipt_rule_set(rule_set_name)
    print(f"Emptying and deleting bucket {bucket_name}.")
    bucket.objects.delete()
    bucket.delete()

    print("Thanks for watching!")
    print('-'*88)
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/ses#code-examples)\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using Amazon SES with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.