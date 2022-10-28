# Create an Amazon SES receipt filter using an AWS SDK<a name="example_ses_CreateReceiptFilter_section"></a>

The following code example shows how to create an Amazon SES receipt filter that blocks incoming mail from an IP address or range of IP addresses\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/ses#code-examples)\. 
  

```
class SesReceiptHandler:
    """Encapsulates Amazon SES receipt handling functions."""
    def __init__(self, ses_client, s3_resource):
        """
        :param ses_client: A Boto3 Amazon SES client.
        :param s3_resource: A Boto3 Amazon S3 resource.
        """
        self.ses_client = ses_client
        self.s3_resource = s3_resource

    def create_receipt_filter(self, filter_name, ip_address_or_range, allow):
        """
        Creates a filter that allows or blocks incoming mail from an IP address or
        range.

        :param filter_name: The name to give the filter.
        :param ip_address_or_range: The IP address or range to block or allow.
        :param allow: When True, incoming mail is allowed from the specified IP
                      address or range; otherwise, it is blocked.
        """
        try:
            policy = 'Allow' if allow else 'Block'
            self.ses_client.create_receipt_filter(
                Filter={
                    'Name': filter_name,
                    'IpFilter': {
                        'Cidr': ip_address_or_range,
                        'Policy': policy}})
            logger.info(
                "Created receipt filter %s to %s IP of %s.", filter_name, policy,
                ip_address_or_range)
        except ClientError:
            logger.exception("Couldn't create receipt filter %s.", filter_name)
            raise
```
+  For API details, see [CreateReceiptFilter](https://docs.aws.amazon.com/goto/boto3/email-2010-12-01/CreateReceiptFilter) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using Amazon SES with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.