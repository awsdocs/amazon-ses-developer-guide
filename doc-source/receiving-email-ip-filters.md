# Creating IP address filters for Amazon SES email receiving<a name="receiving-email-ip-filters"></a>

An IP address filter enables you to optionally specify whether to accept or reject mail originating from an IP address or range of IP addresses\.

You can use the Amazon SES console or the `CreateReceiptFilter` API to create an IP address filter\.

**Note**  
If you only want to receive mail from a finite list of known IP addresses, then set up a block list that contains `0.0.0.0/0`, and set up an allow list that contains the IP addresses that you trust\. This configuration blocks all IP addresses by default, and only allows mail from the IP addresses that you explicitly specify\.

**To create an IP address filter \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **IP Address Filters**\.

1. In the content pane, choose **Create Filter**\.

1. For **Filter Name**, type a name for the IP address filter\. The name must contain less than 64 alphanumeric, hyphen \(\-\), underscore \(\_\), and period \(\.\) characters\. The name must start and end with a letter or number\.

1. For **IP Address Range**, type a single IP address or a range of IP addresses that you want to block or allow, specified in Classless Inter\-Domain Routing \(CIDR\) notation\. An example of a single IP address is 10\.0\.0\.1\. An example of a range of IP addresses is 10\.0\.0\.1/24\. For more information about CIDR notation, see [RFC 2317](https://tools.ietf.org/html/rfc2317)\.

1. For **Policy Type**, choose **Allow** or **Block**\.

1. Choose **Create Filter**\.

For information about how to use the `CreateReceiptFilter` API to create an IP address filter, see the *[Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptFilter.html)*\.
