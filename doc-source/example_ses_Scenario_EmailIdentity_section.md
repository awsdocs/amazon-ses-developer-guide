# Verify and manage Amazon SES identities using an AWS SDK<a name="example_ses_Scenario_EmailIdentity_section"></a>

The following code example shows how to verify and manage Amazon SES identities\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
  

```
def usage_demo():
    print('-'*88)
    print("Welcome to the Amazon Simple Email Service (Amazon SES) identities demo!")
    print('-'*88)

    logging.basicConfig(level=logging.INFO, format='%(levelname)s: %(message)s')

    ses_identity = SesIdentity(boto3.client('ses'))
    email = input(
        "Enter an email address to verify with Amazon SES. This address will "
        "receive a verification email: ")
    ses_identity.verify_email_identity(email)

    print(f"Follow the steps in the email to {email} to complete verification.")
    print("Waiting for verification...")
    try:
        ses_identity.wait_until_identity_exists(email)
        print(f"Identity verified for {email}.")
    except WaiterError:
        print(f"Verification timeout exceeded. You must complete the "
              f"steps in the email sent to {email} to verify the address.")

    identities = ses_identity.list_identities('EmailAddress', 10)
    print("The identities in the account are:")
    print(*identities, sep='\n')

    status = ses_identity.get_identity_status(email)
    print(f"{email} has status: {status}.")

    answer = input(f"Do you want to remove {email} from Amazon SES (y/n)? ")
    if answer.lower() == 'y':
        ses_identity.delete_identity(email)
        print(f"{email} removed from Amazon SES.")

    print("Thanks for watching!")
    print('-'*88)
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/ses#code-examples)\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using Amazon SES with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.