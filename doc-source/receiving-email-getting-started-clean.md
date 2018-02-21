# Step 6: Clean Up<a name="receiving-email-getting-started-clean"></a>

After you have completed this tutorial, you should clean up the following settings and resources to avoid incurring additional charges\.

## Amazon SES Receipt Rule Set<a name="receiving-email-getting-started-clean-ses"></a>

If you no longer want Amazon SES to receive mail for your domain, you must disable your active receipt rule set\.

## Amazon S3 Bucket<a name="receiving-email-getting-started-clean-s3"></a>

If you no longer want the Amazon S3 bucket that you created, you can delete it\. However, you cannot delete an Amazon S3 bucket that has items in it, so you must first delete the contents of the bucket, and then delete the bucket\. For more information about deleting folders and buckets, see [Delete an Object and Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/DeletingAnObjectandBucket.html) in the *Amazon S3 Getting Started Guide*\. 

## Route 53 Domain<a name="receiving-email-getting-started-clean-r53"></a>

If you no longer want Route 53 to register your domain, you can [delete the registration](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-delete.html) or [transfer your domain to another registrar](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-transfer-from-route-53.html)\.