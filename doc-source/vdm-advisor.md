# Virtual Deliverability Manager advisor<a name="vdm-advisor"></a>

The *Virtual Deliverability Manager advisor* helps to optimize your email deliverability and engagement by identifying key performance and infrastructure issues at the account and sending identity levels that are adversely affecting your email deliverability and reputation\. It provides solutions by providing specific guidance on how to resolve the identified issue\.

Advisor’s infrastructure recommendations are listed in the *Open recommendations* table\. The recommendations identify standard email authentication problems, such as a lack of SPF, DKIM, or DMARC records or a key length that's too short \(DKIM\)\. They're categorized by severity of impact, sending domain, and the age of the alert\. In the search bar, a list box provides the option to filter on impact level, infrastructure category, or sending identity name\. The last column, *Resolve issue*, provides a link to the relevant section in the Amazon SES Developer Guide with guidance about how to resolve the identified issue\.

Open recommendations display in the Virtual Deliverability Manager advisor sorted by impact level\.

![\[Open recommendations displayed in the Virtual Deliverability Manager advisor sorted by impact level.\]](http://docs.aws.amazon.com/ses/latest/dg/images/vdm_advisor_overview.png)

If you don't have any ongoing advisor notifications, a message will indicate that you don’t have any open recommendations\. We recommend that you check the advisor on a regular basis\.

You can also access the *Resolved recommendations* table from the Virtual Deliverability Manager advisor page, which lists infrastructure issues that you’ve resolved by implementing the advisor's guidance\. Resolved recommendations are listed with an initial status that describes the issue before it was resolved\. Resolved recommendations expire after 30 days\.

## Using the Virtual Deliverability Manager advisor in the Amazon SES console<a name="vdm-advisor-console"></a>

The following procedure shows you how to use the Virtual Deliverability Manager advisor in the Amazon SES console to resolve identified deliverability issues using the Amazon SES console\.

**To use the Virtual Deliverability Manager advisor to resolve deliverability and reputation issues**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Advisor** under **Virtual Deliverability Manager**\.
**Note**  
**Advisor** will not be visible if you haven't enabled Virtual Deliverability Manager for your account\. For more information, see [Getting started with Virtual Deliverability Manager](vdm-get-started.md)\.

1. The **Open recommendations table** displays by default\. Recommendations are categorized by **Impact** \(High/Low\), **Identity name** \(sending domain\), **Age** \(of the alert\), and **Recommendation/Description** \(identified issue\)\. In the search bar, filter on the **Impact** level, the infrastructure issue **Category**, or the **Identity name** of the sending domain\.

1. To remediate a problem that's described in the **Recommendation/Description** column, choose the link in the **Resolve issue** column for that row, and implement the suggested solution\.
**Note**  
After you implement a solution, the resolved issue can take up to six hours to be reflected\. You can view the resolved issue on the **Resolved recommendations** tab\.

## Accessing your Virtual Deliverability Manager recommendations using the AWS CLI<a name="vdm-advisor-cli"></a>

The following examples show you how to access your Virtual Deliverability Manager recommendations using the AWS CLI\.

**To access your Virtual Deliverability Manager recommendations using the AWS CLI**  
You can use the [https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_ListRecommendations.html](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_ListRecommendations.html) operation in the Amazon SES API v2 to list your deliverability recommendations\. You can call this operation from the AWS CLI, as shown in the following examples\.
+ List the recommendations to see deliverability issues:

  ```
  aws --region us-east-1 sesv2 list-recommendations
  ```
+ Apply filters to retrieve recommendations for a specific domain that you own:

  ```
  aws --region us-east-1 sesv2 list-recommendations --cli-input-json file://list-recommendations.json
  ```
+ The input file looks similar to this:

  ```
  {
    "PageSize":100,
    "Filter":{
      "RESOURCE_ARN": "arn:aws:ses:us-east-1:123456789012:identity/example.com"
     }  
  }
  ```