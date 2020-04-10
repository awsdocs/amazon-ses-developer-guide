# Prerequisites<a name="event-publishing-redshift-prerequisites"></a>

For this tutorial, you will need the following:
+ **An AWS account** – To access any web service that AWS offers, you must first create an AWS account at [https://aws\.amazon\.com/](https://aws.amazon.com/)\.
+ **Verified email address** – To send emails using Amazon SES, you must verify your "From" address or domain to show that you own it\. If you are in the sandbox, you also must verify your "To" addresses\. You can verify email addresses or entire domains, but this tutorial requires a verified email address so that you can send an email from the Amazon SES console, which is the simplest way to send an email\. For information about how to verify an email address, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)\.
+ **A SQL query tool** – Amazon Redshift does not provide or install any SQL client tools or libraries, so you must install one that you can use to access the Amazon Redshift clusters that contain your Amazon SES events\. In this tutorial, we use [SQL Workbench/J](http://www.sql-workbench.net/), a free, DBMS\-independent, cross\-platform SQL query tool\. This section includes procedures for installing SQL Workbench/J\.

**To install SQL Workbench/J**

1. Review the [SQL Workbench/J software license](http://www.sql-workbench.net/manual/license.html#license-restrictions)\.

1. Go to the [SQL Workbench/J website](http://www.sql-workbench.net/) and download the appropriate package for your operating system\.

1. Go to [Installing and starting SQL Workbench/J](http://www.sql-workbench.net/manual/install.html) and install SQL Workbench/J\.
**Important**  
Note the Java runtime version prerequisites for SQL Workbench/J and ensure you are using that version\. Otherwise, this client application will not run\.

1. Go to [Configure a JDBC Connection](https://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-connection.html) and download an Amazon Redshift JDBC driver to enable SQL Workbench/J to connect to your cluster\.

## Next Step<a name="event-publishing-redshift-prerequisites-next-step"></a>

[Step 1: Create an Amazon Redshift Cluster](event-publishing-redshift-cluster.md)