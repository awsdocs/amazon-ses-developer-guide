# Advanced email personalization<a name="send-personalized-email-advanced"></a>

The template feature in Amazon SES is based on the Handlebars template system\. You can use Handlebars to create templates that include advanced features, such as nested attributes, array iteration, basic conditional statements, and the creation of inline partials\. This section provides examples of these features\.

Handlebars includes additional features beyond those documented in this section\. For more information, see [Built\-In Helpers](https://handlebarsjs.com/guide/builtin-helpers.html) at [handlebarsjs\.com](http://handlebarsjs.com)\.

**Topics**
+ [Parsing nested attributes](#send-personalized-email-advanced-nested)
+ [Iterating through lists](#send-personalized-email-advanced-iterating)
+ [Using basic conditional statements](#send-personalized-email-advanced-conditionals)
+ [Creating inline partials](#send-personalized-email-advanced-inline-partials)

## Parsing nested attributes<a name="send-personalized-email-advanced-nested"></a>

Handlebars includes support for nested paths, which makes it easy to organize complex customer data, and then refer to that data in your email templates\.

For example, you can organize recipient data into several general categories\. Within each of those categories, you can include detailed information\. The following code example shows an example of this structure for a single recipient:

```
{
  "meta":{
    "userId":"51806220607"
  },
  "contact":{
    "firstName":"Anaya",
    "lastName":"Iyengar",
    "city":"Bengaluru",
    "country":"India",
    "postalCode":"560052"
  },
  "subscription":[
    {
      "interest":"Sports"
    },
    {
      "interest":"Travel"
    },
    {
      "interest":"Cooking"
    }
  ]
}
```

In your email templates, you can refer to nested attributes by providing the name of the parent attribute, followed by a period \(\.\), followed by the name of the attribute for which you want to include the value\. For example, if you use the data structure shown in the preceding example, and you want to include each recipient's first name in the email template, include the following text in your email template: `Hello {{contact.firstName}}!`

Handlebars can parse paths that are nested several levels deep, which means you have flexibility in how you structure your template data\.

## Iterating through lists<a name="send-personalized-email-advanced-iterating"></a>

The `each` helper function iterates through items in an array\. The following code is an example of an email template that uses the `each` helper function to create an itemized list of each recipient's interests\.

```
{
  "Template": {
    "TemplateName": "Preferences",
    "SubjectPart": "Subscription Preferences for {{contact.firstName}} {{contact.lastName}}",
    "HtmlPart": "<h1>Your Preferences</h1>
                 <p>You have indicated that you are interested in receiving 
                   information about the following subjects:</p>
                 <ul>
                   {{#each subscription}}
                     <li>{{interest}}</li>
                   {{/each}}
                 </ul>
                 <p>You can change these settings at any time by visiting 
                    the <a href=https://www.example.com/prefererences/i.aspx?id={{meta.userId}}>
                    Preference Center</a>.</p>",
    "TextPart": "Your Preferences\n\nYou have indicated that you are interested in 
                 receiving information about the following subjects:\n
                 {{#each subscription}}
                   - {{interest}}\n
                 {{/each}}
                 \nYou can change these settings at any time by 
                 visiting the Preference Center at 
                 https://www.example.com/prefererences/i.aspx?id={{meta.userId}}"
  }
}
```

**Important**  
In the preceding code example, the values of the `HtmlPart` and `TextPart` attributes include line breaks to make the example easier to read\. The JSON file for your template can't contain line breaks within these values\. If you copied and pasted this example into your own JSON file, remove the line breaks and extra spaces from the `HtmlPart` and `TextPart` sections before proceeding\.

After you create the template, you can use the `SendTemplatedEmail` or the `SendBulkTemplatedEmail` operation to send email to recipients using this template\. As long as each recipient has at least one value in the `Interests` object, they receive an email that includes an itemized list of their interests\. The following example shows a JSON file that can be used to send email to multiple recipients using the preceding template:

```
{
  "Source":"Sender Name <sender@example.com>",
  "Template":"Preferences",
  "Destinations":[
    {
      "Destination":{
        "ToAddresses":[
          "anaya.iyengar@example.com"
        ]
      },
      "ReplacementTemplateData":"{\"meta\":{\"userId\":\"51806220607\"},\"contact\":{\"firstName\":\"Anaya\",\"lastName\":\"Iyengar\"},\"subscription\":[{\"interest\":\"Sports\"},{\"interest\":\"Travel\"},{\"interest\":\"Cooking\"}]}"
      },
    {
      "Destination":{ 
        "ToAddresses":[
          "shirley.rodriguez@example.com"
        ]
      },
      "ReplacementTemplateData":"{\"meta\":{\"userId\":\"1981624758263\"},\"contact\":{\"firstName\":\"Shirley\",\"lastName\":\"Rodriguez\"},\"subscription\":[{\"interest\":\"Technology\"},{\"interest\":\"Politics\"}]}"
    }
  ],
  "DefaultTemplateData":"{\"meta\":{\"userId\":\"\"},\"contact\":{\"firstName\":\"Friend\",\"lastName\":\"\"},\"subscription\":[]}"
}
```

When you send an email to the recipients listed in the preceding example using the `SendBulkTemplatedEmail` operation, they receive a message that resembles the example shown in the following image:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/send-personalized-email-advanced-condition-interest.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/)

## Using basic conditional statements<a name="send-personalized-email-advanced-conditionals"></a>

This section builds on the example described in the previous section\. The example in the previous section uses the `each` helper to iterate through a list of interests\. However, recipients for whom no interests are specified receive an email that contains an empty list\. By using the `{{if}}` helper, you can format the email differently if a certain attribute is present in the template data\. The following code uses the `{{if}}` helper to display the bulleted list from the preceding section if the `Subscription` array contains any values\. If the array is empty, a different block of text is displayed\.

```
{
  "Template": {
    "TemplateName": "Preferences2",
    "SubjectPart": "Subscription Preferences for {{contact.firstName}} {{contact.lastName}}",
    "HtmlPart": "<h1>Your Preferences</h1>
                 <p>Dear {{contact.firstName}},</p>
                 {{#if subscription}}
                   <p>You have indicated that you are interested in receiving 
                     information about the following subjects:</p>
                     <ul>
                     {{#each subscription}}
                       <li>{{interest}}</li>
                     {{/each}}
                     </ul>
                     <p>You can change these settings at any time by visiting 
                       the <a href=https://www.example.com/prefererences/i.aspx?id={{meta.userId}}>
                       Preference Center</a>.</p>
                 {{else}}
                   <p>Please update your subscription preferences by visiting 
                     the <a href=https://www.example.com/prefererences/i.aspx?id={{meta.userId}}>
                     Preference Center</a>.
                 {{/if}}",
    "TextPart": "Your Preferences\n\nDear {{contact.firstName}},\n\n
                 {{#if subscription}}
                   You have indicated that you are interested in receiving 
                   information about the following subjects:\n
                   {{#each subscription}}
                     - {{interest}}\n
                   {{/each}}
                   \nYou can change these settings at any time by visiting the 
                   Preference Center at https://www.example.com/prefererences/i.aspx?id={{meta.userId}}.
                 {{else}}
                   Please update your subscription preferences by visiting the 
                   Preference Center at https://www.example.com/prefererences/i.aspx?id={{meta.userId}}.
                 {{/if}}"
  }
}
```

**Important**  
In the preceding code example, the values of the `HtmlPart` and `TextPart` attributes include line breaks to make the example easier to read\. The JSON file for your template can't contain line breaks within these values\. If you copied and pasted this example into your own JSON file, remove the line breaks and extra spaces from the `HtmlPart` and `TextPart` sections before proceeding\.

The following example shows a JSON file that can be used to send email to multiple recipients using the preceding template:

```
{
  "Source":"Sender Name <sender@example.com>",
  "Template":"Preferences2",
  "Destinations":[
    {
      "Destination":{
        "ToAddresses":[
          "anaya.iyengar@example.com"
        ]
      },
      "ReplacementTemplateData":"{\"meta\":{\"userId\":\"51806220607\"},\"contact\":{\"firstName\":\"Anaya\",\"lastName\":\"Iyengar\"},\"subscription\":[{\"interest\":\"Sports\"},{\"interest\":\"Cooking\"}]}"
      },
    {
      "Destination":{ 
        "ToAddresses":[
          "shirley.rodriguez@example.com"
        ]
      },
      "ReplacementTemplateData":"{\"meta\":{\"userId\":\"1981624758263\"},\"contact\":{\"firstName\":\"Shirley\",\"lastName\":\"Rodriguez\"}}"
    }
  ],
  "DefaultTemplateData":"{\"meta\":{\"userId\":\"\"},\"contact\":{\"firstName\":\"Friend\",\"lastName\":\"\"},\"subscription\":[]}"
}
```

In this example, the recipient whose template data included a list of interests receives the same email as the example shown in the previous section\. The recipient whose template data did not include any interests, however, receives an email that resembles the example shown in the following image:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/send-personalized-email-advanced-condition-nointerest.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/)

## Creating inline partials<a name="send-personalized-email-advanced-inline-partials"></a>

You can use inline partials to simplify templates that include repeated strings\. For example, you could create an inline partial that includes the recipient's first name, and, if it is available, their last name by adding the following code to the beginning of your template:

```
{{#* inline \"fullName\"}}{{firstName}}{{#if lastName}} {{lastName}}{{/if}}{{/inline}}\n
```

**Note**  
The newline character \(`\n`\) is required to separate the `{{inline}}` block from the content in your template\. The newline isn't rendered in the final output\.

After you create the `fullName` partial, you can include it anywhere in your template by preceding the name of the partial with a greater\-than \(>\) sign followed by a space, as in the following example: `{{> fullName}}`\. Inline partials are not transferred between parts of the email\. For example, if you want to use the same inline partial in both the HTML and the text version of the email, you must define it in both the `HtmlPart` and the `TextPart` sections\.

You can also use inline partials when iterating through arrays\. You can use the following code to create a template that uses the `fullName` inline partial\. In this example, the inline partial applies to both the recipient's name and to an array of other names:

```
{
  "Template": {
    "TemplateName": "Preferences3",
    "SubjectPart": "{{firstName}}'s Subscription Preferences",
    "HtmlPart": "{{#* inline \"fullName\"}}
                   {{firstName}}{{#if lastName}} {{lastName}}{{/if}}
                 {{/inline~}}\n
                 <h1>Hello {{> fullName}}!</h1>
                 <p>You have listed the following people as your friends:</p>
                 <ul>
                 {{#each friends}}
                   <li>{{> fullName}}</li>
                 {{/each}}</ul>",
    "TextPart": "{{#* inline \"fullName\"}}
                   {{firstName}}{{#if lastName}} {{lastName}}{{/if}}
                 {{/inline~}}\n
                 Hello {{> fullName}}! You have listed the following people 
                 as your friends:\n
                 {{#each friends}}
                   - {{> fullName}}\n
                 {{/each}}"
  }
}
```

**Important**  
In the preceding code example, the values of the `HtmlPart` and `TextPart` attributes include line breaks to make the example easier to read\. The JSON file for your template can't contain line breaks within these values\. If you copied and pasted this example into your own JSON file, remove the line breaks and extra spaces from these sections\.
