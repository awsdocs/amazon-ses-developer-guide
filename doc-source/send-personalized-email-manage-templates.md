# Managing email templates<a name="send-personalized-email-manage-templates"></a>

In addition to [creating email templates](send-personalized-email-api.md), you can also use the Amazon SES API to update or delete existing templates, to list all of your existing templates, or to view the contents of a template\. 

This section contains procedures for using the AWS CLI to perform tasks related to Amazon SES templates\.

**Note**  
The procedures in this section assume that you've already installed and configured the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

## Viewing a list of email templates<a name="send-personalized-email-manage-templates-list"></a>

You can use the [ListTemplates](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListTemplates.html) operation in the Amazon SES API to view a list of all of your existing email templates\.

**To view a list of email templates**
+ At the command line, enter the following command:

  ```
  aws ses list-templates
  ```

  If there are existing email templates in your Amazon SES account in the current Region, this command returns a response that resembles the following example:

  ```
  {
      "TemplatesMetadata": [
          {
              "Name": "SpecialOffers",
              "CreatedTimestamp": "2020-08-05T16:04:12.640Z"
          },
          {
              "Name": "NewsAndUpdates",
              "CreatedTimestamp": "2019-10-03T20:03:34.574Z"
          }
      ]
  }
  ```

  If you haven't created any templates, the command returns a `TemplatesMetadata` object with no members\.

## Viewing the contents of a specific email template<a name="send-personalized-email-manage-templates-get"></a>

You can use the [GetTemplate](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetTemplate.html) operation in the Amazon SES API to view the contents of a specific email template\.

**To view the contents of an email template**
+ At the command line, enter the following command:

  ```
  aws ses get-template --template-name MyTemplate
  ```

  In the preceding command, replace *MyTemplate* with the name of the template that you want to view\.

  If the template name that you provided matches a template that exists in your Amazon SES account, this command returns a response that resembles the following example:

  ```
  {
      "Template": {
          "TemplateName": "TestMessage",
          "SubjectPart": "Amazon SES Test Message",
          "TextPart": "Hello! This is the text part of the message.",
          "HtmlPart": "<html>\n<body>\n<h2>Hello!</h2>\n<p>This is the HTML part of the message.</p></body>\n</html>"
      }
  }
  ```

  If the template name that you provided doesn't match a template that exists in your Amazon SES account, the command returns a `TemplateDoesNotExist` error\.

## Deleting an email template<a name="send-personalized-email-manage-templates-delete"></a>

You can use the [DeleteTemplate](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteTemplate.html) operation in the Amazon SES API to delete a specific email template\.

**To delete an email template**
+ At the command line, enter the following command:

  ```
  aws ses delete-template --template-name MyTemplate
  ```

  In the preceding command, replace *MyTemplate* with the name of the template that you want to delete\.

  This command doesn't provide any output\. You can verify that the template was deleted by using the [GetTemplate](#send-personalized-email-manage-templates-get) operation\.

## Updating an email template<a name="send-personalized-email-manage-templates-update"></a>

You can use the [UpdateTemplate](https://docs.aws.amazon.com/ses/latest/APIReference/API_UpdateTemplate.html) operation in the Amazon SES API to update an existing email template\. For example, this operation is helpful if you want to change the subject line of the email template, or if you need to modify the body of the message itself\.

**To update an email template**

1. Use the `GetTemplate` command to retrieve the existing template by entering the following command on the command line:

   ```
   aws ses get-template --template-name MyTemplate
   ```

   In the preceding command, replace *MyTemplate* with the name of the template that you want to update\.

   If the template name that you provided matches a template that exists in your Amazon SES account, this command returns a response that resembles the following example:

   ```
   {
       "Template": {
           "TemplateName": "TestMessage",
           "SubjectPart": "Amazon SES Test Message",
           "TextPart": "Hello! This is the text part of the message.",
           "HtmlPart": "<html>\n<body>\n<h2>Hello!</h2>\n<p>This is the HTML part of the message.</p></body>\n</html>"
       }
   }
   ```

1. In a text editor, create a new file\. Paste the output of the previous command into the file\.

1. Modify the template as needed\. Any lines that you omit are removed from the template\. For example, if you only want to change the `SubjectPart` of the template, you still need to include the `TextPart` and `HtmlPart` properties\.

   When you finish, save the file as `update_template.json`\.

1. At the command line, enter the following command:

   ```
   aws ses update-template --cli-input-json file://path/to/update_template.json
   ```

   In the preceding command, replace *path/to/update\_template\.json* with the path to the `update_template.json` file that you created in the previous step\.

   If the template is updated successfully, this command doesn't provide any output\. You can verify that the template was updated by using the [GetTemplate](#send-personalized-email-manage-templates-get) operation\.

   If the template that you specified doesn't exist, this command returns a `TemplateDoesNotExist` error\. If the template doesn't contain either the `TextPart` or `HtmlPart` property \(or both\), this command returns an `InvalidParameterValue` error\. 
