# Create and manage Amazon SES templates using an AWS SDK<a name="example_ses_Scenario_Templates_section"></a>

The following code example shows how to create and manage Amazon SES templates\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
Call functions from the API examples section to create and manage email templates\.  

```
def usage_demo():
    print('-'*88)
    print("Welcome to the Amazon Simple Email Service (Amazon SES) email template "
          "demo!")
    print('-'*88)

    logging.basicConfig(level=logging.INFO, format='%(levelname)s: %(message)s')

    ses_template = SesTemplate(boto3.client('ses'))
    template = {
        'name': 'doc-example-template',
        'subject': 'Example of an email template.',
        'text': "This is what {{name}} will {{action}} if {{name}} can't display HTML.",
        'html': "<p><i>This</i> is what {{name}} will {{action}} if {{name}} "
                "<b>can</b> display HTML.</p>"}
    print("Creating an email template.")
    ses_template.create_template(**template)
    print("Getting the list of template metadata.")
    template_metas = ses_template.list_templates()
    for temp_meta in template_metas:
        print(f"Got template {temp_meta['Name']}:")
        temp_data = ses_template.get_template(temp_meta['Name'])
        pprint(temp_data)
    print(f"Deleting template {template['name']}.")
    ses_template.delete_template()

    print("Thanks for watching!")
    print('-'*88)
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/ses#code-examples)\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using Amazon SES with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.