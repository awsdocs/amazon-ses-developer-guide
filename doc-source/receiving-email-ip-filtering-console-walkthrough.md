# Create IP address filters console walkthrough<a name="receiving-email-ip-filtering-console-walkthrough"></a>

This section will walk you through setting up IP address filters using the Amazon SES console\. IP address filtering allows you to provide a broad level of control\. These IP filters allow you to explicitly block or allow all messages from specific IP addresses or IP address ranges\.

Optionally, you can use the `CreateReceiptFilter` API to create an IP address filter as described in the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptFilter.html)\.

**Note**  
If you only want to receive mail from a finite list of known IP addresses, then set up a block list that contains `0.0.0.0/0`, and set up an allow list that contains the IP addresses that you trust\. This configuration blocks all IP addresses by default, and only allows mail from the IP addresses that you explicitly specify\.

## Prerequisites<a name="ip-filtering-prerequisites"></a>

The following prerequisites must be met before proceeding with setting up recipient based email control using IP address filters: 

1. You first need to [create and verify a domain identity](verify-addresses-and-domains.md) in Amazon SES\.

1. Next, you need to specify which mail servers can accept mail for your domain by [publishing an MX record](receiving-email-mx-record.md) to your domain's DNS settings\. \(The MX record should refer to the Amazon SES endpoint that receives mail for the AWS Region where you use Amazon SES\.\)

## Create IP address filters<a name="receipt-rules-create-ip-filters"></a>

**To create IP address filters using the console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Email receiving**\.

1. Select the **IP address filters** tab\.

1. Choose **Create Filter**\.

1. Enter an unique name for your filter \- the field's legend will indicate syntax requirements\. \(The name must contain less than 64 alphanumeric, hyphen \(\-\), underscore \(\_\), and period \(\.\) characters\. The name must start and end with a letter or number\.\)

1. Enter an IP address or a range of IP addresses \- the field's legend will give examples specified in Classless Inter\-Domain Routing \(CIDR\) syntax\. \(An example of a single IP address is 10\.0\.0\.1\. An example of a range of IP addresses is 10\.0\.0\.1/24\. For more information about CIDR notation, see [RFC 2317](https://tools.ietf.org/html/rfc2317)\.\)

1. Choose the **Policy type** by selecting either the **Block** or **Allow** radio button\.

1. Choose **Create filter**\.

1. If you want to add another IP filter, choose **Create filter** and repeat the previous steps for each additional filter you wish to add\.

1. If you want to remove an IP address filter, select it and choose the **Delete** button\.