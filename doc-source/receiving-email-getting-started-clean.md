# Step 6: Clean Up<a name="receiving-email-getting-started-clean"></a>

After you complete this tutorial, you can clean up the resources you created to avoid incurring additional charges\.

## Amazon SES Receipt Rule Set<a name="receiving-email-getting-started-clean-ses"></a>

If you no longer want Amazon SES to receive mail for your domain, you can [disable the active receipt rule set](receiving-email-managing-receipt-rule-sets.md#receiving-email-managing-receipt-rule-sets-enable-disable)\.

## Amazon S3 Bucket<a name="receiving-email-getting-started-clean-s3"></a>

If you no longer want the Amazon S3 bucket that you created, you can delete it\. To delete a bucket, you must first delete its contents\. For more information about deleting folders and buckets, see [Delete an Object and Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/DeletingAnObjectandBucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\.

## Route 53 Domain<a name="receiving-email-getting-started-clean-r53"></a>

If you no longer want to use Route 53 to register your domain, you can [delete the registration](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-delete.html) or [transfer the domain to another registrar](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-transfer-from-route-53.html)\.